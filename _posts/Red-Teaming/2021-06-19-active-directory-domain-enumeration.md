---
title:  "Active Directory Domain Enumeration Part-1 With Powerview"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
categories:
  - Red-Teaming
toc: true
---

Enumeration is the process of extracting information from the Active Directory (users and groups)

## Install PowerView
  Download script
 - https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerView/powerview.ps1

  ```. ./powerview.ps1
  ```

<img src="/img/adpart1/1.png" alt="Getting-gz" width="800" height="110"> 

we notice that Powerview detected 

disable protection

```
Set-MpPreference -DisableRealtimeMonitoring $true
```


## Unpacking

