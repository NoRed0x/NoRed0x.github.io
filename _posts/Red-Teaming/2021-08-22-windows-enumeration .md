---
title:  "Windows Enumeration "
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Windows Enumeration"
categories:
  - Red-Teaming
toc: true
---

if you are interested in network penetration tester or red teaming 
I collected most of the commands used in windows enumeration,enjoy 

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
Hostname prints the name of the PC you are currently connected to.

whoami:tells you the domain and the username of the user currently connected as
```
hostname
whoami 
set computername
```

<img src="/img/win_enum/3.PNG" alt="Getting-gz" width="600" height="200"> 


## Network
Ipconfig displays all the networking information of the current PC your connected to.
```
ipconfig
```

<img src="/img/win_enum/n2.PNG" alt="Getting-gz" width="800" height="300"> 

If you add a /all to the ipconfig command it will give you a more detailed output which includes the DHCP and DNS server that the PC is connected to.
```
ipconfig /all
```

<img src="/img/win_enum/n3.PNG" alt="Getting-gz" width="800" height="300"> 

```
ipconfig /allcompartments /all
```

<img src="/img/win_enum/4.PNG" alt="Getting-gz" width="800" height="300"> 
 
  ```
 wmic nicconfig get description,IPAddress,MACaddress
 ```
 
<img src="/img/win_enum/5.PNG" alt="Getting-gz" width="800" height="300"> 

route print command displays the routing table of the current windows PC your connected to
```
route PRINT
```

<img src="/img/win_enum/6.PNG" alt="Getting-gz" width="800" height="300"> 


The arp -a command displays the IP to physical address translation used by the address resolution protocol.
```
arp -a
```

<img src="/img/win_enum/7.PNG" alt="Getting-gz" width="800" height="300"> 

netstat is a command-line network utility that displays network connections for Transmission Control Protocol, routing tables, and a number of network interface and network protocol
```
netstat
netstat -ano
```

<img src="/img/win_enum/8.PNG" alt="Getting-gz" width="800" height="300"> 




## firewall configuration
netsh firewall gives you options to control the windows firewall
```
netsh advfirewall show currentprofile
```

<img src="/img/win_enum/f1.PNG" alt="Getting-gz" width="800" height="300"> 

```
netsh advfirewall firewall show rule name=all
```
 
<img src="/img/win_enum/f2.PNG" alt="Getting-gz" width="800" height="300"> 

```
netsh firewall show state
```

<img src="/img/win_enum/f4.PNG" alt="Getting-gz" width="800" height="300"> 

```
netsh firewall show config
```

<img src="/img/win_enum/f5.PNG" alt="Getting-gz" width="800" height="300"> 

## windows defender
```
sc query windefend
```

<img src="/img/win_enum/f3.PNG" alt="Getting-gz" width="800" height="300"> 

## running processes
Tasklist displays a list of currently running processes on a PC.
```
tasklist /SVC
```

<img src="/img/win_enum/2020.PNG" alt="Getting-gz" width="800" height="300"> 

## Is the machine on a domain?
```
set userdomain
```

<img src="/img/win_enum/9.PNG" alt="Getting-gz" width="600" height="200"> 



## Registry
Windows autologin
```
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
```

<img src="/img/win_enum/req1.PNG" alt="Getting-gz" width="600" height="200"> 

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
```
wmic bios
```
<img src="/img/win_enum/h1.PNG" alt="Getting-gz" width="1000" height="200"> 

```
wmic baseboard get manufacturer
```

<img src="/img/win_enum/222.PNG" alt="Getting-gz" width="1000" height="200"> 

```
wmic cpu list full
```
<img src="/img/win_enum/h2.PNG" alt="Getting-gz" width="1000" height="200"> 

## patches are installed
qfe to the wmic command you get a list of all the installed hotfixes installed on a windows PC.
What patches are installed?
```
wmic qfe
```

<img src="/img/win_enum/pa.PNG" alt="Getting-gz" width="1000" height="200"> 



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

## Unquoted Service Paths
Find Services With Unquoted Paths

```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
```

## Net Start
allows you to manage services running on the PC
```
net start
```

<img src="/img/win_enum/n1.PNG" alt="Getting-gz" width="800" height="200"> 


## device drivers
Driverquery quickly displays all the device drivers of the Pc your connected to.
```
Driverquery
```

<img src="/img/win_enum/dq.PNG" alt="Getting-gz" width="1000" height="300"> 

## scheduled tasks
Schtasks allows you to manage scheduled tasks running on a local or remote machine

```
Schtasks /query /fo LIST /v
```

<img src="/img/win_enum/st.PNG" alt="Getting-gz" width="1000" height="300"> 

## device&kernal

```
Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}
```

<img src="/img/win_enum/p1.PNG" alt="Getting-gz" width="1000" height="300"> 

## Cleartext Passwords
```
findstr /si password *.txt
findstr /si password *.xml
findstr /si password *.ini
```

## Find all those strings in config files.
```
dir /s *pass* == *cred* == *vnc* == *.config*
```

## Find all passwords in all files.
```
findstr /spin "password" *.*
findstr /spin "password" *.*
```

I finished this part about windows enumeration today waiting me in the next part.

