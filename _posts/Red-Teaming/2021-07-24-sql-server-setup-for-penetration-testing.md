---
title:  "sql server setup for penetration testing "
classes: wide
header:
  teaser: /img/sqlserver.jpg
ribbon: red
description: "sql server setup for penetration testing"
categories:
  - Red-Teaming
toc: true
---
how to install and configure MS SQL server in windows server
## Requirement
## Download setup file SQLEXPR_x64_ENU.exe
```
https://download.microsoft.com/download/4/1/A/41AD6EDE-9794-44E3-B3D5-A1AF62CD7A6F/sql16_sp2_dlc/en-us/SQLEXPR_x64_ENU.exe
```
## Download setup file SQLManagementStudio_x86_ENU.exe
```
https://download.microsoft.com/download/f/e/b/feb0e6be-21ce-4f98-abee-d74065e32d0a/SSMS-Setup-ENU.exe
```

## Configure SQL express setup
Click on installation >> New SQL Server standalone installation

<img src="/img/sqlserver/1.png" alt="Getting-gz" width="800" height="200"> 

 accept the license terms 
 
<img src="/img/sqlserver/2.PNG" alt="Getting-gz" width="800" height="200"> 

<img src="/img/sqlserver/3.PNG" alt="Getting-gz" width="800" height="200"> 

it will start installing SQL server Rules file on your system which takes some time

<img src="/img/sqlserver/4.PNG" alt="Getting-gz" width="800" height="200"> 

## Feature Selection
enabled check for 
  * Database Engine service
  * SQL Server Replication
  * SQL Client Connective SDK

<img src="/img/sqlserver/5.PNG" alt="Getting-gz" width="800" height="200"> 

## Instance Configuration
the name and instance ID for instance of SQL server

<img src="/img/sqlserver/6.PNG" alt="Getting-gz" width="800" height="200"> 

SQL Server Browser Startup type Automatic.

<img src="/img/sqlserver/7.PNG" alt="Getting-gz" width="800" height="200"> 

## Database Engine Configuration
 * lick on mixed mode which is a combination of both type authentication SQL Server and Windows.
 * Type your password and confirm the password for the administrator account.
 
<img src="/img/sqlserver/8.PNG" alt="Getting-gz" width="800" height="200"> 

SQL server 2016 installation completed successfully

<img src="/img/sqlserver/9.PNG" alt="Getting-gz" width="800" height="200"> 

## SQL server configuration manager 
open the SQL server configuration manager 
  * SQL server network configuration
  * protocol for SQL Express
  * tcp/ip
  
<img src="/img/sqlserver/10.PNG" alt="Getting-gz" width="800" height="200"> 


<img src="/img/sqlserver/11.PNG" alt="Getting-gz" width="800" height="200"> 

Under IP Addresses specify TCP port 1433 tab, Click on Apply and Enable the TCP/IP.

<img src="/img/sqlserver/12.PNG" alt="Getting-gz" width="800" height="200"> 


## Configure SQL Management Studio setup
 open 2nd downloaded application for SQL server management setup >>install it

<img src="/img/sqlserver/13.PNG" alt="Getting-gz" width="800" height="200"> 

Now login in to SQL Server using admin credential 

<img src="/img/sqlserver/14.PNG" alt="Getting-gz" width="800" height="200"> 

Right Click on SQLEXPRESS( SQL Server) and go to Facets
 
<img src="/img/sqlserver/15.PNG" alt="Getting-gz" width="800" height="200"> 

go to General tab left side, then on the right side explore the Facet and select Surface Area Configuration

<img src="/img/sqlserver/16.PNG" alt="Getting-gz" width="800" height="200"> 

select True on XPCmdShellEnabled
 
<img src="/img/sqlserver/17.PNG" alt="Getting-gz" width="800" height="200"> 

new login account for other users.

<img src="/img/sqlserver/18.PNG" alt="Getting-gz" width="800" height="200"> 

choosing SQL server authentication for this user
 
<img src="/img/sqlserver/19.PNG" alt="Getting-gz" width="800" height="200"> 

## Connect to server from windows 10
```
ip :192.168.128.145
user :nored0x
password:p@ssw0rd
port:1433
```
## HeidiSQL 
  * is a useful and reliable tool designed for web developers using the popular MySQL server, Microsoft SQL databases, and PostgreSQL
  * It enables you to browse and edit data, create and edit tables, views, procedures, triggers, and scheduled events.

## download HeidiSQL
```
https://www.heidisql.com/download.php
```

<img src="/img/sqlserver/20.PNG" alt="Getting-gz" width="800" height="200"> 

We have successfully accessed the database system of the MSSQL server

<img src="/img/sqlserver/21.PNG" alt="Getting-gz" width="800" height="200"> 

I finished part 1 in sql server today waite me in the next part.


