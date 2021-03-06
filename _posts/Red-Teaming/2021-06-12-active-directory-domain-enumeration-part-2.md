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
<img src="/img/adpart2/ds.png" alt="Getting-gz" width="800" height="200"> 

Find all domain shares in the current domain that the current user has read access to.
```
Find-DomainShare -CheckShareAccess
```
<img src="/img/adpart2/dsc.png" alt="Getting-gz" width="800" height="200"> 


Obtains the file server used by the current domain according to the SPN
 ```
 Get-NetFileServer -Verbose
 ```
 
 <img src="/img/adpart2/fs.png" alt="Getting-gz" width="800" height="200"> 

## files
It will search for sensitive files such as the Credentials files and other files that can lead to a serious compromise

Find sensitive files on computer in the domain
```
Invoke-FileFinder
```

<img src="/img/adpart2/2.PNG" alt="Getting-gz" width="800" height="200"> 


## Group Policies
 Group Policy provides the ability to manage configuration and changes easily and centrally in AD Allows configuration of Security settings,Registry-based policy settings. 
 
 Group policy preferences like startup/shutdown/log-on/logoff scripts settings and Software installation. 
 GPO can be abused for various attacks like privesc, backdoors, persistence etc
 
 Get list of GPO in the current domain
 ```
 Get-NetGPO
 ```
 
<img src="/img/adpart2/gpo1.png" alt="Getting-gz" width="800" height="230"> 
 
```
Get-NetGPO| select displayname
```
 
<img src="/img/adpart2/gpo2.png" alt="Getting-gz" width="500" height="100"> 

 
Get list of GPO in the target computer
```
Get-NetGPO -ComputerName <ComputerName> | select displayname
```
 
<img src="/img/adpart2/gpo3.png" alt="Getting-gz" width="800" height="200"> 

 
Find users who have local admin rights over the machine
```
Find-GPOComputerAdmin ???Computername <ComputerName>
```
 
Get machines where the given user in member of a specific group 
```
Find-GPOLocation -Identity <user> -Verbose
```
 <img src="/img/adpart2/gpo4.png" alt="Getting-gz" width="800" height="250"> 

 
## OUs
OUs are the smallest unit in the Active Directory system
OU is abbreviated from is Organizational Unit
OUs are containers for users, groups, and computers, and they exist within a domain
OUs are useful when an administrator wants to deploy Group Policy settings to a subset of users, groups, and computers within your domain
OUs also allows Administrators to delegate admin tasks to users/groups without having to make him/her an administrator of the directory

Get all the OUs in the current domain

```
Get-NetOU
```

<img src="/img/adpart2/ou1.png" alt="Getting-gz" width="800" height="200"> 

<img src="/img/adpart2/ou2.png" alt="Getting-gz" width="500" height="100"> 

## ACLs
An Access Control List (ACL) is a list of access control entries (ACE). Each ACE in an ACL identifies a trustee and specifies the access rights allowed, denied, or audited for that trustee. 

The security descriptor for a securable object can contain two types of ACLs: a DACL and a SACL.
 * DACL(Discretionary access control list): Defines the permissions trustees (a user or group) have on an object.
 * SACL(System access control list): Logs success and failure audit messages when an object is accessed.

* the ACL is like asking: who has permission and what can be done on an object?
* Most of the system administrators are wrongly configuring the ACL (such as granting a normal user to important permissions). So as attackers, we are interested in enumerating   the ACL in order to find interesting ACLs!


 Enumerate the ACLs for the users group
 ```
 Get-ObjectAcl -SamAccountName "users" -ResolveGUIDs
 ```

<img src="/img/adpart2/ac1.PNG" alt="Getting-gz" width="800" height="200"> 

see if there is any user has a modification rights to a GPO
```
Get-NetGPO | %{Get-ObjectAcl -ResolveGUIDs -Name $_.Name}
```

<img src="/img/adpart2/2ac.PNG" alt="Getting-gz" width="800" height="200"> 

search for interesting ACE
```
Invoke-ACLScanner -ResolveGUIDs
```
  
<img src="/img/adpart2/3ac.PNG" alt="Getting-gz" width="800" height="250"> 

Returns the ACLs associated with the specified account
```
Get-DomainObjectAcl -Identity <user> -ResolveGUIDs
```
<img src="/img/adpart2/4ac.PNG" alt="Getting-gz" width="700" height="200"> 

 
Get the ACL associated with the specific path
```
Get-PathAcl -Path "\\10.0.0.2\Users"
```

<img src="/img/adpart2/5ac.PNG" alt="Getting-gz" width="600" height="150"> 

I finished part 2 today waite me in the next part.

