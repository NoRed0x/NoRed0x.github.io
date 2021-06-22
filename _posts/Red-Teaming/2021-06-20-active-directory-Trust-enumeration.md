---
title:  "Active Directory Domain Trust and forest Enumeration"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "enumerating Domain Trust and forest  in Active Directory "
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

## relationship trust

  <img src="/img/ad3/type.PNG" alt="Getting-gz" width="800" height="200"> 


  Parent/Child Trust 
 * A transitive, two-way parent-child trust relationship automatically created and establishes a relationship between a parent domain and a child domain whenever a new child domain is created using the AD DS installation process process within a domain tree. 
 * They can only exist between two domains in the same tree with the same contiguous namespace. 
 * The parent domain is always trusted by the child domain. You cannot manually create a Parent-Child trust.
  
  Tree/Root Trust
 * A transitive, two-way tree-root trust relationship automatically created and establishes a relationship between the forest root domain and a new tree, when you run the AD DS installation process to add a new tree to the forest. 
 * A tree-root trust can only be established between the roots of two trees in the same forest and are always transitive. You cannot manually create a tree-root trust
  
  Shortcut Trust
 * Shortcut trusts are manually created, one-way, transitive trusts.
 * They can only exist within a forest. 
 * They are created to optimize the authentication process shortening the trust path. 
 * The trust path is the series of domain trust relationships that the authentication process must traverse between two domains in a forest that are not directly trusted by each other. Shortcut trusts shorten the trust path

  Forest Trust 
 * Forest trusts are manually created, one-way transitive, or two-way transitive trusts that allow you to provide access to resources between multiple forests. 
 * Forest trusts uses both Kerberos v5 and NTLM authentication across forests where users can use their Universal Principal Name (UPN)
 
  Realm Trust 
 * Trust Relationships with Other Operating Systems that also Support Kerberos Protocol 
 * One-Way Transitive or One-Way Non-Transitive Use Kerberos Authentication Only 
 * A Realm trust can be established to provide resource access and cross-platform inter-operability between an AD DS domain and non-Windows Kerberos v5 Realm.

  External Trust 
 * An external trust is a one-way, non-transitive trust that is manually created to establish a trust relationship between AD DS domains that are in different forests, or between an AD DS domain and Windows NT 4.0 domain.
 *  External trusts allow you to provide users access to resources in a domain outside of the forest that is not already trusted by a Forest trust.




Get list of all domain trust for the currrent domain
```
Get-NetDomainTrust
Get-NetDomainTrust -Domain <domain>
Get-DomainTrustMapping
```

## forest

get details about the current forest
```
Get-NetForest
```

<img src="/img/ad3/f1.png" alt="Getting-gz" width="800" height="200"> 

Get details about the other forest

```
Get-NetForest -Forest <forest>
```

<img src="/img/ad3/f2.png" alt="Getting-gz" width="800" height="200"> 

get all domain in the current forest
```
Get-NetForestDomain
```

<img src="/img/ad3/f3.png" alt="Getting-gz" width="800" height="200"> 

```
 Get-NetForestDomain -Forest karim.net
 ```
<img src="/img/ad3/f4.png" alt="Getting-gz" width="800" height="200"> 


## global catalog 
 is a feature of Active Directory  domain controllers that allows for a domain controller to provide information on any object in the forest, regardless of whether the object is a member of the domain controller’s domain.

Get all global catalogs for the current forest
```
Get-NetForestCatalog 
```
<img src="/img/ad3/f5.png" alt="Getting-gz" width="800" height="200"> 

```
 Get-NetForestCatalog -Forest <forest>
   ```

<img src="/img/ad3/f6.png" alt="Getting-gz" width="800" height="200"> 


