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

<img src="/img/adpart2/2.PNG" alt="Getting-gz" width="800" height="200"> 

Find-DomainShare -CheckShareAccess

## Group Policies
 Group Policy provides the ability to manage configuration and changes easily and centrally in AD ,Allows configuration of Security settings,Registry-based policy settings. 
 
 Group policy preferences like startup/shutdown/log-on/logoff scripts settings and Software installation. 
 GPO can be abused for various attacks like privesc, backdoors, persistence etc
 
 get list of GPO in the current domain
 ```
 Get-NetGPO
 
 ```
 
 <img src="/img/adpart2/gpo1.PNG" alt="Getting-gz" width="800" height="200"> 
 
 ```
 Get-NetGPO| select displayname
 
 ```
 
<img src="/img/adpart2/gpo2.PNG" alt="Getting-gz" width="800" height="200"> 

 
 get list of GPO in the target computer
```
Get-NetGPO -ComputerName <ComputerName> | select displayname
```
 
 <img src="/img/adpart2/gpo3.PNG" alt="Getting-gz" width="800" height="200"> 

 
 find users who have local admin rights over the machine
 ```
 Find-GPOComputerAdmin –Computername <ComputerName>
 ```
 
 get machines where the given user in member of a specific group 
 ```
 Find-GPOLocation -Identity <user> -Verbose
 ```
 <img src="/img/adpart2/gpo4.PNG" alt="Getting-gz" width="800" height="200"> 

 
## OUs
OUs are the smallest unit in the Active Directory system. OU is abbreviated from is Organizational Unit. OUs are containers for users, groups, and computers, and they exist within a domain. OUs are useful when an administrator wants to deploy Group Policy settings to a subset of users, groups, and computers within your domain. OUs also allows Administrators to delegate admin tasks to users/groups without having to make him/her an administrator of the directory

Get all the OUs in the current domain
```
Get-NetOU
```

<img src="/img/adpart2/ou1.PNG" alt="Getting-gz" width="800" height="200"> 

<img src="/img/adpart2/ou2.PNG" alt="Getting-gz" width="800" height="200"> 

## ACLs




















