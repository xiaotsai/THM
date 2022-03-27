# task 1

列出目標系統上的用戶。其中一個類似於一面旗幟。 先用`net users` 可以看到使用者 
THM-17213
目標機器的操作系統版本是多少？ 然後`systeminfo`

10.0.17763 N/A Build 17763
何時安裝安全更新 KB4562562？
```
補丁級別
Microsoft 會定期發布適用於 Windows 系統的更新和補丁。目標系統上缺少的關鍵補丁可能是一個容易被利用的提權票。下面的命令可用於列出目標系統上安裝的更新。

wmic qfe get Caption,Description,HotFixID,InstalledOn



WMIC 是 Windows 上的命令行工具，它為 Windows Management Instrumentation (WMI) 提供接口。WMI用於Windows上的管理操作，是一個值得了解的強大工具。WMIC 可以提供比僅安裝的補丁更多的信息。例如，它可用於查找我們將在後續任務中看到的未引用服務路徑漏洞。WMIC在 Windows 10 版本 21H1 和 Windows Server 的 21H1 半年頻道版本中已棄用。對於較新的 Windows 版本，您將需要使用 WMI PowerShell cmdlet。
```

Windows Defender 的狀態是什麼？
您可以採取兩種方法：專門查找防病毒軟件或列出所有正在運行的服務並檢查哪些服務可能屬於防病毒軟件。



第一種方法可能需要事先進行一些研究，以了解有關防病毒軟件使用的服務名稱的更多信息。比如Windows系統默認安裝的殺毒軟件，Windows Defender的服務名稱是windefend。下面的查詢將搜索名為“windefend”的服務並返回其當前狀態。

`sc query windefend`



雖然第二種方法可以讓您在事先不知道其服務名稱的情況下檢測防病毒軟件，但輸出可能是壓倒性的。

`sc queryex type=service`

# task 2 
兩個app一個一個去文件夾點開看
然後
```
┌──(kali㉿kali)-[~]
└─$ searchsploit fitbit
--------------------------------------------------- ---------------------------------
 Exploit Title                                     |  Path
--------------------------------------------------- ---------------------------------
Fitbit Connect Service - Unquoted Service Path Pri | windows/local/40482.txt
```
