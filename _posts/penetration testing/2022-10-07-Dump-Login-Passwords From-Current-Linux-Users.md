---
title:  "Dump Login Passwords From Current Linux Users"
classes: wide
header:
  teaser: /img/linux.jpg
ribbon: MidnightBlue
categories:
  - Penetration Testing 
toc: true
---


Dump Login Passwords From Current Linux Users

## mimipenguin 
MimiPenguin works similarly to the well-known "mimikatz" for Windows, but is designed for Linux and attempts to dump cleartext credentials from memory from the following applications

• Apache2 (Active HTTP Basic Auth Sessions)
• OpenSSH (Active SSH Sessions - Sudo Usage)
• GDM password (Kali Desktop, Debian Desktop)
• Gnome Keyring (Ubuntu Desktop, ArchLinux Desktop)
• VSFTPd (Active FTP Connections)


Takes advantage of cleartext credentials in memory by dumping the process and extracting lines that have a high probability of containing cleartext passwords. Will attempt to calculate each word's probability by checking hashes in /etc/shadow, hashes in memory, and regex searches 





## install mimipenguin

```
git clone https://github.com/huntergregal/mimipenguin
cd mimipenguin/
./mimipenguin.sh 
```




<img src="/img/ad3/f2.png" alt="Getting-gz" width="800" height="200"> 
