---
title:  "Active Directory Domain Enumeration Part-1 With Powerview"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
categories:
  - Red-Teaming
---


#### Description

# Infection Flow

QBot can be delivered in various different ways including Malspam (Malicious Spam) or dropped by other malware families like Emotet.

The infection flow for this campaign is as follows:

[![1](/assets/images/malware-analysis/qbot-banking-trojan/1.png)](/assets/images/malware-analysis/qbot-banking-trojan/1.png)

[![3](/assets/images/malware-analysis/qbot-banking-trojan/3.png)](/assets/images/malware-analysis/qbot-banking-trojan/3.png)

The VBS file tries to download Qbot from different places:

- http://st29[.]ru/tbzirttmcnmb/88888888.png
- http://restaurantbrighton[.]ru/uyqcb/88888888.png
- http://royalapartments[.]pl/vtjwwoqxaix/88888888.png
- http://alergeny.dietapacjenta[.]pl/pgaakzs/88888888.png
- http://egyorg[.]com/vxvipjfembb/88888888.png

Notice the misleading URL, it looks like it's downloading a PNG image but the raw data says something else.

# Unpacking

QBot is packed with a custom packer, but the unpacking process is really simple. It allocates memory for the unpacked code using `VirtualAlloc()` and changes memory protection using `VirtualProtect()`. So we just need 2 breakpoints at  `VirtualAlloc()`  and  `VirtualProtect()`.
idc.set_cmt(ea, dec, 1)


```python
idx = 0
while idx < 0x36F4:
    dec = decrypt_string(idx)
    idx += len(dec)+1
    print(dec)
```

# Anti-Analysis

QBot spawns a new process of itself with the  `"/C"` parameter, this process is responsible for doing Anti-Analysis checks.
So let's go over the anti-analysis techniques.
