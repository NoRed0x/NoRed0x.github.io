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

<img src="/img/win_enum/1.PNG" alt="Getting-gz" width="800" height="300"> 

os version
```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

<img src="/img/win_enum/2.PNG" alt="Getting-gz" width="800" height="300"> 


## Hostname

```
hostname
whoami
```

<img src="/img/win_enum/3.PNG" alt="Getting-gz" width="800" height="300"> 


## Network

## firewall configuration

## Is the machine on a domain?

## Software

## User Info

## Registry


# Hardware Information



































