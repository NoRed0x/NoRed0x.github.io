---
title:  "Active Directory Domain Enumeration Part-1 With Powerview"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
categories:
  - Red-Teaming
---

QBot can be delivered in various different ways including Malspam (Malicious Spam) or dropped by other malware families like Emotet.

# Infection Flow

QBot can be delivered in various different ways including Malspam (Malicious Spam) or dropped by other malware families like Emotet.

The infection flow for this campaign is as follows:

The VBS file tries to download Qbot from different places:

Notice the misleading URL, it looks like it's downloading a PNG image but the raw data says something else.

# Unpacking

QBot is packed with a custom packer, but the unpacking process is really simple. It allocates memory for the unpacked code using `VirtualAlloc()` and changes memory protection using `VirtualProtect()`. So we just need 2 breakpoints at  `VirtualAlloc()`  and  `VirtualProtect()`.
idc.set_cmt(ea, dec, 1)
