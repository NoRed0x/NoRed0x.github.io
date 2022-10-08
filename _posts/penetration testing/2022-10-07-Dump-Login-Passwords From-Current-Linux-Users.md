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

 <img src="/img/88.png" alt="Getting-gz" width="800" height="150"> 



## dumping credentials from memory

we can also dump sensitive information from the swap file.
As everything is a "file" in linux, so is swap space, and we can use that to our advantage using built-in tools. 

The partition or “file” defined as the swap file can be found with the following commands:

`swapon -s`

 <img src="/img/0d.png" alt="Getting-gz" width="800" height="150"> 


we can see that our swap partition is at /dev/sda5

We can obtain the exact same information by issuing the “cat”
command to the “/proc/swaps” file:

`cat /proc/swaps`

 <img src="/img/1d.png" alt="Getting-gz" width="800" height="150"> 



We can use the strings command against the /dev/sda5 

`strings /dev/sda5 |grep "password=" `


## swap_digger.sh

swap_digger is a bash script used to automate Linux swap analysis for post-exploitation or forensics purpose. It automates swap extraction and searches for Linux user credentials, Web form credentials, Web form emails, HTTP basic authentication, WiFi SSID and keys, etc.

```
git clone https://github.com/sevagas/swap_digger.git
cd swap_digger
chmod +x swap_digger.sh
sudo ./swap_digger.sh -vx
```

