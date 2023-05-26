---
title:  "Injured Android solutions "
classes: wide
header:
  teaser: /img/lala.PNG
ribbon: MidnightBlue
categories:
  - Penetration Testing 
toc: true
---

Injured Android solutions

## mimipenguin 

MimiPenguin works similarly to the well-known "mimikatz" for Windows, but is designed for Linux and attempts to dump cleartext credentials from memory from the following applications

* Apache2 (Active HTTP Basic Auth Sessions)
* OpenSSH (Active SSH Sessions - Sudo Usage)
* GDM password (Kali Desktop, Debian Desktop)
* Gnome Keyring (Ubuntu Desktop, ArchLinux Desktop)
* VSFTPd (Active FTP Connections)


Takes advantage of cleartext credentials in memory by dumping the process and extracting lines that have a high probability of containing cleartext passwords. Will attempt to calculate each word's probability by checking hashes in /etc/shadow, hashes in memory, and regex searches 


## install mimipenguin

```
git clone https://github.com/huntergregal/mimipenguin
cd mimipenguin/
./mimipenguin.sh 
```

 <img src="/img/1.PNG" alt="Getting-gz" width="800" height="150"> 
 
 
  <img src="/img/2.PNG" alt="Getting-gz" width="800" height="150"> 
  
   <img src="/img/3.PNG" alt="Getting-gz" width="800" height="150"> 
   
    <img src="/img/4.PNG" alt="Getting-gz" width="800" height="150"> 


