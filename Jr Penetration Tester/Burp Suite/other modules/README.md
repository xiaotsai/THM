# Sequencer
Sequencer 是在 CTF 和其他實驗室環境中很少使用的工具之一，但它是真實世界 Web 應用程序滲透測試的重要組成部分。        
Sequencer 允許我們測量“令牌”的熵（或換句話說，隨機性）    

例如，我們可能希望分析會話 cookie 或保護表單提交的跨站點請求偽造( CSRF )令牌的隨機性。如果事實證明這些token不是安全生成的，那麼我們可以（理論上）預測即將到來的token的價值。
## 使用兩種主要方法來使用 Sequencer 執行令牌分析：
```
實時捕獲是這兩種方法中更常見的一種——這是 Sequencer 的默認子選項卡。實時捕獲允許我們將請求傳遞給 Sequencer，我們知道它將創建一個令牌供我們分析。例如，我們可能希望將 POST 請求傳遞到登錄端點到 Sequencer，因為我們知道服務器將通過給我們一個 cookie 來響應。傳入請求後，我們可以告訴 Sequencer 開始實時捕獲：然後它將自動發出數千次相同的請求，存儲生成的令牌樣本以供分析。一旦我們積累了足夠的樣本，我們就會停止 Sequencer 並允許它分析捕獲的令牌。
------------------------------------------------------------------
手動加載允許我們將預先生成的令牌樣本列表直接加載到 Sequencer 中進行分析。使用手動加載意味著我們不必向目標發出數千個請求（這既吵鬧又耗費資源），但這確實意味著我們需要獲取大量預先生成的令牌！
```
捕獲到一個請求。右鍵單擊請求並選擇“Send to Sequencer”   
請注意，在“響應中的令牌位置”部分中，我們可以選擇在 Cookie、表單字段和自定義位置之間進行選擇。在本例中，我們正在測試loginToken，因此選擇“表單域”的單選按鈕：      

單擊“開始實時捕獲”按鈕！    

現在將彈出一個新窗口，告訴我們我們正在執行實時捕獲，並向我們顯示到目前為止我們捕獲了多少令牌。我們需要等到我們捕獲了合理數量的代幣（大約 10,000 個應該可以）；我們擁有的代幣越多，我們的分析就越準確。    

捕獲大約 10,000 個令牌後，單擊“暫停”，然後選擇“立即分析”按鈕   

注意：我們也可以選擇“停止”捕獲；但是，通過選擇暫停它，如果報告沒有足夠的樣本來準確計算令牌的熵，我們會讓選項恢復捕獲打開。

## 分析
Burp 對其捕獲的令牌樣本執行數十次測試。我們不會考慮所有這些，因為這樣做需要的不僅僅是一項任務（而且將每種技術分開會變得非常數學密集）。相反，我們將專注於生成的摘要；但是，我們鼓勵您自己查看所有測試結果。

生成的熵分析報告分為四個主要部分 - 其中第一個是結果摘要：
```
總結給了我們一個整體的結果；有效熵；分析結果的可靠性；以及所取樣本的摘要。

總的來說，這些通常足以確定令牌是否安全生成；但是，在某些情況下，我們可能需要直接查看測試結果——這可以在“字符級分析”和“位級分析”選項卡中完成。如前所述，為了避免在適合初學者的房間中深入研究統計分析數學，我們不會深入探討這些內容。簡而言之，根據所提供的數據，估計有 1% 的錯誤可能性（顯著性水平：1%），Burp 計算出我們代幣的有效熵應該大約為 117 位。這是安全令牌中具有的極好的熵水平，儘管應該注意的是，僅僅由於主題的性質，不可能絕對保證這種計算是完全準確的。

回答以下問題
花一些時間查看 Burp 用來生成摘要的測試。您不需要了解所有這些，但重要的是要知道它們的存在。
無需回答

```
