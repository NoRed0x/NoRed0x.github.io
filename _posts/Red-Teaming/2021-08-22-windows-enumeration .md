---
title:  "Windows Enumeration Part-1 "
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Windows Enumeration"
categories:
  - Red-Teaming
toc: true
---


## Operating System
What version of windows is running? Is it 32 or 64-bit?
```
ver
wmic os get osarchitecture
```

<img src="/img/win_enum/1.PNG" alt="Getting-gz" width="600" height="200"> 

os version
```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

<img src="/img/win_enum/2.PNG" alt="Getting-gz" width="800" height="300"> 


## Hostname

```
hostname
whoami
set computername
```

<img src="/img/win_enum/3.PNG" alt="Getting-gz" width="600" height="200"> 


## Network
```
ipconfig /allcompartments /all
```

<img src="/img/win_enum/4.PNG" alt="Getting-gz" width="800" height="300"> 
 
 
 ```
 wmic nicconfig get description,IPAddress,MACaddress
 ```
 
 <img src="/img/win_enum/5.PNG" alt="Getting-gz" width="800" height="300"> 

```
route PRINT
```

<img src="/img/win_enum/6.PNG" alt="Getting-gz" width="800" height="300"> 


arp is a command-line network utility that  manipulates the System's ARP cache. It also allows a complete dump of the ARP cache
```
arp -a
```

<img src="/img/win_enum/7.PNG" alt="Getting-gz" width="800" height="300"> 

netstat is a command-line network utility that displays network connections for Transmission Control Protocol, routing tables, and a number of network interface and network protocol
```
netstat
```

<img src="/img/win_enum/8.PNG" alt="Getting-gz" width="800" height="300"> 




## firewall configuration
```
netsh advfirewall show currentprofile
```

<img src="/img/win_enum/f1.PNG" alt="Getting-gz" width="800" height="300"> 

```
netsh advfirewall firewall show rule name=all
```
 
<img src="/img/win_enum/f2.PNG" alt="Getting-gz" width="800" height="300"> 

```
sc query windefend
```

<img src="/img/win_enum/f3.PNG" alt="Getting-gz" width="800" height="300"> 

## running process
```
tasklist /SVC
```

<img src="/img/win_enum/2020.PNG" alt="Getting-gz" width="800" height="300"> 


## Is the machine on a domain?

```
set userdomain
```

<img src="/img/win_enum/9.PNG" alt="Getting-gz" width="600" height="200"> 


## Software

## User Info

## Registry
```
reg query "HKLM\Software\Microsoft\Windows NT\Currentversion\Winlogon" /v LastUsedUsername
```


<img src="/img/win_enum/10.PNG" alt="Getting-gz" width="800" height="300"> 

# Search for password in registry
```
reg query HKLM /f password /t REG_SZ /s

```

<img src="/img/win_enum/r1.PNG" alt="Getting-gz" width="800" height="300"> 

```
reg query HKCU /f password /t REG_SZ /s
```

<img src="/img/win_enum/r2.PNG" alt="Getting-gz" width="800" height="200"> 



# Hardware Information




## install app

```
 wmic product get name, version, vendor
 ```

<img src="/img/win_enum/i1.PNG" alt="Getting-gz" width="1000" height="300"> 


```
 wmic qfe get Caption, Description, HotFixID, InstalledOn
 ```



<img src="/img/win_enum/i2.PNG" alt="Getting-gz" width="1000" height="300"> 

```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows"
```

<img src="/img/win_enum/i3.PNG" alt="Getting-gz" width="1000" height="300"> 
























