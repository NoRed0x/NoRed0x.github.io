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
  

## Active Directory trust
trust is a relationship between two domains or forests which allows users of one domain or forest to access resources in the other domain or forest

In simplest terms, it is the process of extending the security boundary of an AD domain or forest to include another AD domain or forest.

## Trust Direction 
 * One-way trust(Unidirectional):Users in the trusted domain can access resources in the trusting domain but the reverse is not true. 
  
  <img src="/img/ad3/1.PNG" alt="Getting-gz" width="800" height="200"> 

 * Two-way trust(Bidirectional):Users of both domains can access resources in the other domain
  
  <img src="/img/ad3/2.PNG" alt="Getting-gz" width="800" height="200"> 
  

## Trust Transitivity 
 Transitive 
 * Can be extended to establish trust relationships with other domains.
 * All the default intra-forest trust relationships (Tree-root, Parent-Child) between domains within a same forest are transitive two-way trusts
            
 Nontransitive 
 * Cannot be extended to other domains in the forest. Can be two-way or one-way.
 * This is the default trust (called external trust) between two domains in different forests when forests do not have a trust relationship. 


<img src="/img/adpart2/.PNG" alt="Getting-gz" width="800" height="200"> 
