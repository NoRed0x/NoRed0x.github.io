---
title:  "Active Directory Domain Trust Enumeration"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "enumerating Domain Trust in Active Directory "
categories:
  - Red-Teaming
toc: true
---

## Overview
Active Directory follows a clear hierarchy, from top to bottom. In that hierarchy are: forests, trees, and domains.
 * Forests:represent the complete Active Directory instance, and are logical containers made up of domain trees, domains, and organizational units.
 * Trees:are collections of domains within the same DNS namespace; these include child domains.
 * Domains:are logical groupings of network objects such as computers, users, applications, and devices on the network such as printers.
 * 
## Active Directory trust
* trust is a relationship between two domains or forests which allows users of one domain or forest to access resources in the other domain or forest
* In simplest terms, it is the process of extending the security boundary of an AD domain or forest to include another AD domain or forest.

<img src="/img/adpart2/.PNG" alt="Getting-gz" width="800" height="200"> 
