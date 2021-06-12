---
title:  "Active Directory Domain Enumeration Part-2 With Powerview"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "enumerating the Shares,Group Policies, OUs, ACLs ,User Hunting and local groups"
categories:
  - Red-Teaming
toc: true
---

## Shares

Find  share on hosts in the current domain
```
Invoke-ShareFinder  -Verbose
```


<img src="/img/adpart2/1.PNG" alt="Getting-gz" width="800" height="200"> 

Get all network shares in the current domain

```
Get-NetShare
```
 <img src="/img/adpart2/5.PNG" alt="Getting-gz" width="800" height="200"> 


## Domian shares
Find all domain shares in the current domain.
```
Find-DomainShare 
```
<img src="/img/adpart2/ds.PNG" alt="Getting-gz" width="800" height="200"> 

Find all domain shares in the current domain that the current user has read access to.
```
Find-DomainShare -CheckShareAccess
```
<img src="/img/adpart2/dsc.PNG" alt="Getting-gz" width="800" height="200"> 


Obtains the file server used by the current domain according to the SPN
 ```
 Get-NetFileServer -Verbose
 ```
 
 <img src="/img/adpart2/fs.PNG" alt="Getting-gz" width="800" height="200"> 

## files
It will search for sensitive files such as the Credentials files and other files that can lead to a serious compromise

Find sensitive files on computer in the domain
```
Invoke-FileFinder
```

<img src="/img/adpart1/9.PNG" alt="Getting-gz" width="800" height="200"> 

Find-DomainShare -CheckShareAccess

## Group Policies
ldv;,wlevv
ewvwv
## OUs


## ACLs
