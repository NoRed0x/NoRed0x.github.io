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


## Enumeration Domain
Get Current Domain
```
 Get-Domain
```

<img src="/img/adpart1/2.png" alt="Getting-gz" width="800" height="200"> 

Enumerate Other Domains
```
Get-Domain -Domain <DomainName>
```

<img src="/img/adpart1/3.png" alt="Getting-gz" width="800" height="200"> 

Get Domain SID
```
Get-DomainSID
```

<img src="/img/adpart1/4.png" alt="Getting-gz" width="800" height="200"> 

Get Domain Policy
```
Get-DomainPolicy
```

<img src="/img/adpart1/5.png" alt="Getting-gz" width="800" height="200"> 


policy configurations of the Domain about system access
```
(Get-DomainPolicy)."SystemAccess"
```

<img src="/img/adpart1/6.png" alt="Getting-gz" width="800" height="200"> 



policy configurations of the Domain about  kerberos
```
 (Get-DomainPolicy)."kerberospolicy"
```

<img src="/img/adpart1/7.png" alt="Getting-gz" width="800" height="200"> 
















