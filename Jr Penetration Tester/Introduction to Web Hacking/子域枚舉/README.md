# task1

三種不同的子域枚舉方法
```
Brute Force
OSINT (Open-Source Intelligence)
Virtual Host.
```
# task2

SSL/TLS 證書

當 CA（證書頒發機構）為域創建 SSL/TLS（安全套接字層/傳輸層安全）證書時，CA 會參與所謂的“證書透明度 (CT) 日誌”。這些是為域名創建的每個 SSL/TLS 證書的可公開訪問的日誌。證書透明度日誌的目的是阻止惡意和意外製作的證書被使用。我們可以利用這項服務來發現屬於某個域的子域，像 https://crt.sh 和 https://transparencyreport.google.com/https/certificates這樣的網站 提供了一個可搜索的證書數據庫，顯示當前和歷史結果。



進入crt.sh，搜索域名tryhackme.com，找到2020-12-26登錄的條目，輸入下面的域名回答問題。

Q: 2020-12-26 登錄 crt.sh 的域是什麼？   
A: store.tryhackme.com   

# task3
搜索引擎

搜索引擎包含數万億個鏈接，指向超過 10 億個網站，這是尋找新子域的絕佳資源。 在 Google 等網站上使用高級搜索方法，例如 site:filter，可以縮小搜索結果的範圍。例如，“-site:www.domain.com site:*.domain.com”將只包含指向域名 domain.com 的結果，但不包括指向 www.domain.com 的任何鏈接；因此，它只向我們顯示屬於 domain.com 的子域名。



轉到Google並使用搜索詞-site:www.tryhackme.com site:*.tryhackme.com，這應該會顯示 tryhackme.com 的子域；使用該子域來回答以下問題。

```
google:   site:*.tryhackme.com
```
```
Q: 使用上述 Google 搜索發現的以B 開頭的 TryHackMe 子域是什麼？    
A: blog.tryhackme.com
```

# task4
Bruteforce DNS（域名系統）枚舉是從常用子域的預定義列表中嘗試數十、數百、數千甚至數百萬個不同的可能子域的方法。由於此方法需要許多請求，因此我們使用工具將其自動化以加快處理速度。在本例中，我們使用名為 dnsrecon 的工具來執行此操作。點擊“查看站點”按鈕打開靜態站點，點擊“運行DNSrecon請求”按鈕開始模擬，然後回答下面的問題。
```
A: api.acmeitsupport.thm
```

# task5
使用 Sublist3r 實現自動化
# task6
某些子域並不總是託管在可公開訪問的DNS結果中，例如 Web 應用程序的開發版本或管理門戶。相反，DNS 記錄可以保存在私有 DNS 服務器上，或者記錄在開發人員機器上的 /etc/hosts 文件（或 Windows 用戶的 c:\windows\system32\drivers\etc\hosts 文件）中，該文件將域名映射到IP 地址。 


因為當客戶端請求網站時，Web 服務器可以從一個服務器託管多個網站，所以服務器從 Host 標頭中知道客戶端想要哪個網站。我們可以通過對其進行更改並監視響應來查看我們是否發現了一個新網站來利用此主機標頭。



與DNS Bruteforce 一樣，我們可以通過使用常用子域的詞彙表來自動化此過程。


```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.119.14
```
使用-w開關來指定我們要使用的詞表。-H開關添加/編輯一個標頭（在本例中為 Host 標頭），我們在子域通常所在的空間中有FUZZ關鍵字，這是我們將嘗試單詞列表中所有選項的地方。

因為上面的命令總是會產生一個有效的結果，所以我們需要過濾輸出。我們可以通過使用帶有-fs開關的頁面大小結果來做到這一點。編輯以下命令，將 {size} 替換為上一個結果中出現次數最多的大小值


```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.119.14 -fs {size}

-fs開關，它告訴 ffuf 忽略任何指定大小的結果。
```


```
Q: 發現的第一個子域是什麼？
A: delta

Q: 發現的第二個子域是什麼？
A: yellow

```