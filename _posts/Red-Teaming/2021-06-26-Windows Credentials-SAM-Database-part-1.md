---
title:  "Windows Credentials-SAM Database part-1"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "Windows Credentials-SAM Database part-1"
categories:
  - Red-Teaming
toc: true
---

## Introduction to SAM 
Local Windows credentials are stored in the Security Account Manager (SAM) database as password hashes using the NTLM hashing format, which is based on the MD4 algorithm.

We can reuse acquired NTLM hashes to authenticate to a different machine, as long as the hash is tied to a user account and password registered on that machine

It is the responsibility of LSA (Local Security Authority) to verify user login by matching the passwords with the database maintained in SAM.

SAM starts running in the background as soon as the Windows boots up.

located at C:\Windows\System32\config\SAM
  
  <img src="/img/ad3/.PNG" alt="Getting-gz" width="800" height="200"> 
 
 
## Decrypting Hash
John The Ripper
