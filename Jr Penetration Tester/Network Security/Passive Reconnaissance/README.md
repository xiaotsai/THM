# Passive Reconnaissance

## task1 

我們使用whois查詢 WHOIS 記錄，同時使用nslookup查詢digDNS數據庫記錄。這些都是公開的記錄，因此不會提醒目標。

```
whois 查詢 WHOIS 服務器
nslookup 查詢DNS服務器
dig 查詢DNS服務器
```


## task2 

將偵察分為：

1. 被動偵察
2. 主動偵察

`被動偵察中，您依賴於公開可用的知識。這是您無需直接與目標接觸即可從公開可用資源中獲取的知識。把它想像成你是從遠處看著目標區域，而不是踏上那個區域。`

```
例如：

從公共 DNS 服務器查找域的DNS記錄。
檢查與目標網站相關的招聘廣告。
閱讀有關目標公司的新聞文章。
```

`主動偵察不能如此謹慎地實現。它需要與目標直接接觸。把它想像成你檢查門窗上的鎖，以及其他潛在的入口點`

```
例子包括：

連接到公司服務器之一，例如HTTP、FTP 和 SMTP。
致電公司以獲取信息（社會工程）。
冒充修理工進入公司場所。

```


## task3

### whois

```
whois tryhackme.com
   Domain Name: TRYHACKME.COM
   Registry Domain ID: 2282723194_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.namecheap.com
   Registrar URL: http://www.namecheap.com
   Updated Date: 2021-05-01T19:43:23Z
   Creation Date: 2018-07-05T19:46:15Z
   Registry Expiry Date: 2027-07-05T19:46:15Z
   Registrar: NameCheap, Inc.
   Registrar IANA ID: 1068
   Registrar Abuse Contact Email: abuse@namecheap.com
   Registrar Abuse Contact Phone: +1.6613102107
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Name Server: KIP.NS.CLOUDFLARE.COM
   Name Server: UMA.NS.CLOUDFLARE.COM
   DNSSEC: unsigned

```

```
TryHackMe.com 是什麼時候註冊的？
20180705

TryHackMe.com 的註冊商是什麼？
namecheap.com

TryHackMe.com 將哪家公司用於名稱服務器？
CLOUDFLARE.COM
```

## task4

### nslokup & dig

```
dig thmlabs.com TXT             

; <<>> DiG 9.17.19-3-Debian <<>> thmlabs.com TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19892
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;thmlabs.com.                   IN      TXT

;; ANSWER SECTION:
thmlabs.com.            300     IN      TXT     "THM{a5b83929888ed36acb0272971e438d78}"

;; Query time: 51 msec
;; SERVER: 192.168.50.1#53(192.168.50.1) (UDP)
;; WHEN: Wed Jan 19 05:21:32 EST 2022
;; MSG SIZE  rcvd: 90

```

`DNS Dumpster`
```
remote
```
## task6
`shodan.io`

```
According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers?

Germany
Based on Shodan.io, what is the 3rd most common port used for Apache?

8080
Based on Shodan.io, what is the 3rd most common port used for nginx?

8888

```


## task7 
```
目的										命令行示例
查找 WHOIS 記錄					whois tryhackme.com
查找DNS A 記錄					nslookup -type=A tryhackme.com
在 DNS 服務器上查找DNS MX 記錄		nslookup -type=MX tryhackme.com 1.1.1.1
查找DNS TXT 記錄					nslookup -type=TXT tryhackme.com
查找DNS A 記錄					dig tryhackme.com A
在 DNS 服務器上查找DNS MX 記錄		dig @1.1.1.1 tryhackme.com MX
查找DNS TXT 記錄					dig tryhackme.com TXT
```