---
title:  "Active Directory Domain Enumeration Part-1 With Powerview"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
categories:
  - Red-Teaming
toc: true
---

Enumeration is the process of extracting information from the Active Directory (users and groups)

## Install PowerView
  Download script
 - https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerView/powerview.ps1

```
. ./powerview.ps1
```

<img src="/img/adpart1/1.png" alt="Getting-gz" width="800" height="110"> 

we notice that Powerview detected 
disable protection

```
Set-MpPreference -DisableRealtimeMonitoring $true
```


## Domain
Get Current Domain
```
 Get-Domain
```

<img src="/img/adpart1/2.PNG" alt="Getting-gz" width="800" height="200"> 

Enumerate Other Domains
```
Get-Domain -Domain <DomainName>
```

<img src="/img/adpart1/3.PNG" alt="Getting-gz" width="800" height="200"> 

Get Domain SID
```
Get-DomainSID
```

<img src="/img/adpart1/4.PNG" alt="Getting-gz" width="800" height="200"> 

Get Domain Policy
```
Get-DomainPolicy
```

<img src="/img/adpart1/5.PNG" alt="Getting-gz" width="800" height="200"> 


policy configurations of the Domain about system access
```
(Get-DomainPolicy)."SystemAccess"
```

<img src="/img/adpart1/6.PNG" alt="Getting-gz" width="800" height="200"> 



policy configurations of the Domain about  kerberos
```
 (Get-DomainPolicy)."kerberospolicy"
```

<img src="/img/adpart1/7.PNG" alt="Getting-gz" width="800" height="200"> 

## Domain Controllers
A domain controller is a server that responds to authentication requests and verifies users on computer networks
```
 Get-DomainController 
 ```
 
 <img src="/img/adpart1/8.PNG" alt="Getting-gz" width="800" height="200"> 
 
 ```
  Get-DomainController -Domain <DomainName>
  ```
  
  <img src="/img/adpart1/9.PNG" alt="Getting-gz" width="800" height="200"> 
  
  
##  Domain Users
A domain user is one whose username and password are stored on a domain controller rather than the computer the user is logging into. When you log in as a domain user, the computer asks the domain controller what privileges are assigned to you. When the computer receives an appropriate response from the domain controller, it logs you in with the proper permissions and restrictions.

Get Domain User 
```
Get-DomainUser 
```

 <img src="/img/adpart1/10.PNG" alt="Getting-gz" width="800" height="200"> 

```
Get-DomainUser | select cn
```

<img src="/img/adpart1/11.PNG" alt="Getting-gz" width="800" height="200"> 

list of all properities for users















