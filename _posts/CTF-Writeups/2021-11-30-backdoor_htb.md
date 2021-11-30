---
title:  "Hackthebox-Backdoor"
classes: wide
header:
  teaser: /assets/images/ctf-writeups/htb/htb.png
ribbon: MidnightBlue
categories:
  - CTF Writeups
toc: true
---


#### Description
* Let’s start with this machine.
* Download the VPN pack for the individual user and use the guidelines to log into the HTB VPN.
* The Backdoor machine is IP is 10.10.11.125

respect me on hack the box  [`https://www.hackthebox.eu/profile/251876`](https://www.hackthebox.eu/profile/251876)


# Recon 
First use nmap for port scanning

## nmap 
```
nmap -Pn -sV -sC -p- 10.10.11.125 -oN backdoor_nmap
```
Nmap shows 3 open ports
  -8080 open, running Tomcat



<img src="/img/backdoor/1nmap.PNG" alt="Getting-gz" width="800" height="440">

## browser
I installed  wappalyzer plugin to show what technology website used ,the target machine uses WordPress 5.8.1 CMS

<img src="/img/backdoor/2brow.PNG.PNG" alt="Getting-gz" width="800" height="440">

## wpscan
```
wpscan --url 10.10.11.125 --enumerate vp,u,vt,tt
```

```
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.17
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: http://10.10.11.125/ [10.10.11.125]
[+] Started: Tue Nov 30 19:28:02 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.41 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.11.125/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://10.10.11.125/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://10.10.11.125/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.11.125/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.8.1 identified (Insecure, released on 2021-09-09).
 | Found By: Rss Generator (Passive Detection)
 |  - http://10.10.11.125/index.php/feed/, <generator>https://wordpress.org/?v=5.8.1</generator>
 |  - http://10.10.11.125/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.8.1</generator>

[+] WordPress theme in use: twentyseventeen
 | Location: http://10.10.11.125/wp-content/themes/twentyseventeen/
 | Latest Version: 2.8 (up to date)
 | Last Updated: 2021-07-22T00:00:00.000Z
 | Readme: http://10.10.11.125/wp-content/themes/twentyseventeen/readme.txt
 | Style URL: http://10.10.11.125/wp-content/themes/twentyseventeen/style.css?ver=20201208
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 2.8 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.10.11.125/wp-content/themes/twentyseventeen/style.css?ver=20201208, Match: 'Version: 2.8'

[+] Enumerating Vulnerable Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Vulnerable Themes (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:00:23 <========================================================================================> (358 / 358) 100.00% Time: 00:00:23
[+] Checking Theme Versions (via Passive and Aggressive Methods)

[i] No themes Found.

[+] Enumerating Timthumbs (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:02:54 <======================================================================================> (2575 / 2575) 100.00% Time: 00:02:54

[i] No Timthumbs Found.

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:04 <==========================================================================================> (10 / 10) 100.00% Time: 00:00:04

[i] User(s) Identified:

[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Wp Json Api (Aggressive Detection)
 |   - http://10.10.11.125/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Tue Nov 30 19:31:46 2021
[+] Requests Done: 3002
[+] Cached Requests: 8
[+] Data Sent: 837.136 KB
[+] Data Received: 18.224 MB
[+] Memory used: 225.582 MB
[+] Elapsed time: 00:03:44
```
## searchsploit
if you didn't find searchsploit ,install it
```
sudo git clone https://github.com/offensive-security/exploitdb.git /opt/exploitdb
sudo ln -sf /opt/exploitdb/searchsploit /usr/local/bin/searchsploit
```
<img src="/img/backdoor/install.PNG.PNG" alt="Getting-gz" width="800" height="440">

it was found that there are three vulnerabilities, but they are all plug-in vulnerabilities.

<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
<img src="/img/backdoor/search.PNG" alt="Getting-gz" width="800" height="440">
