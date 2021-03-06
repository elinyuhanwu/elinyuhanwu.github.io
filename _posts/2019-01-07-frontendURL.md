---
layout: post
title: "從輸入URL到頁面加載中間發生了什麼？"
subtitle: "\"What happens when you type google.com into your browser's address box and press enter?\""
author: "Han"
header-img: "img/post-bg-google.jpeg"
header-mask: 0.3
tags:
  - 前端
  - 筆記
---
> 最近在準備面試新的前端工作，看到了這個題目，就當作Blog中第一篇前端文章，以作為自己基礎知識梳理習慣的開始

在前端的世界中，每天都有新的框架、新的設計、新的想法，但沒有了基礎知識的全盤瞭解，很容易在運用新技術時無法順利上手，遇到Bug卻不得解，我深知自己並非計算機相關科系專業，遇到這種在程序之前的題目，更需要花時間學習了解，關於這個題目的文章網路上有很多，這篇文章主要是我自己的一些學習筆記、重點整理，也先為之後準備寫的PWA鋪程。


# 主要回答
1. 瀏覽器查找域名對應的 IP 地址

2. 瀏覽器根據 IP 地址與服務器建立 socket 連接

3. 瀏覽器與服務器通信： 瀏覽器請求，服務器處理請求

4. 瀏覽器與服務器斷開連接


# 其他重點
1. 從瀏覽器接收 url 到開啟網絡請求線程 ( 展開瀏覽器的機制以及進程與線程之間的關係, 不同瀏覽器內核渲染流程不同 )

2. 開啟網絡線程到發出一個完整的 http 請求 ( DNS Lookup, tcp/ip 請求...... )

3. 從服務器接收到請求到對應後台接收到請求 ( 負載均衡,安全攔截以及後台內部處理等等 )

4. 後台與前台的 http 交互 ( http header, 響應碼, 報文結構, cookie等知識, 靜態資源的cookie優化以及編碼解碼, 如gzip壓縮等 )

5. 緩存問題： http 的緩存 ( http緩存header, etag, catch-control等 )

6. 瀏覽器接收到 http 數據包後的解析流程 ( 解析html -> DOM Tree, 解析CSS -> CSS Rule Tree, 合併成Render Tree, layout, painting, reflow, > repaint, 復合塗層的合成, GPU繪製, 外鍊資源的處理, loaded和domContentLoaded )

7. CSS的可視化格式模型 ( 元素的渲染規則, 如包含塊, 控制框, BFC, IFC等概念 )

8. JS引擎解析過程 ( JS的解釋階段, 預處理階段, 執行階段生成執行上下文, VO, 作用域鍊, 回收機制等等 )

9. 其它 ( 跨域, web安全, hybrid模式...... )


## 從瀏覽器接收url到開啟網絡請求線程

### 瀏覽器的多進程
瀏覽器是多進程的，有一個主控進程，以及每個標籤頁面都會新開一個進程（某些情況下多個標籤頁面會合併進程）

進程可能包括主動進程，插件進程，GPU，標籤頁（Rendering Engine）等等

* Browser進程：瀏覽器的主進程（負責協調、主控），只有一個
* 第三方插件進程：每種類型的插件對應一個進程，僅當使用該插件時才創建
* GPU進程：最多一個，用於3D繪製
* 瀏覽器渲染進程(瀏覽器內核)：默認每個標籤頁面一個進程，多線程在其中，互不影響，控制頁面渲染，腳本執行，事件處理等（有時候會優化，如多個空白標籤頁會合併成一個進程）


### 多線程的瀏覽器內核 Rendering Engine
每一個標籤頁面可以看作是瀏覽器內核進程，是多線程的，它有幾大類子線程

* GUI(圖形用戶介面)渲染線程
* JS 引擎線程
* 事件觸發線程
* 定時器線程
* 網絡請求線程: 異步http請求線程

### 解析URL (Parse URL)
URL分為以下幾個部分：
>`protocol` 協議頭，譬如http，ftp等
>
>`host` 主機域名或IP位置
>
>`port` 端口號
>
>`path` 目錄路徑
>
>`query` 即查詢參數
>
>`fragment` 即#後的hash值，一般用來定位到某個位置

## 開啟網絡線程到發出HTTP請求
這一階段過程如下，大致上包含了DNS Lookup，ARP Process，TCP/IP請求構建

### Check HSTS list
瀏覽器檢驗是否為 HSTS(HTTP Strict Transport Security)，如果是瀏覽器發出`https`請求而非`http`。

### DNS 查詢獲得 IP
1. 瀏覽器搜索自己的DNS緩存

2. 搜索操作系統中的DNS緩存

3. 搜索操作系統中的hosts文件

4. 操作系統將域名發送至LDNS，LDNS查找自己的DNS緩存，成功返回結果，失敗則發起一個迭代DNS解析請求

    1) LDNS 向Root Name Server 發起請求，Root Name Server 返回com域的頂級域名服務器的地址

    2) LDNS 向com域的頂級域名服務器發起請求，返回域名服務器地址

    3) LDNS 向域名服務器發起請求，得到IP地址

5. LDNS將得到的IP地址返回給操作系統，同時也將IP位置緩存起來

6. 操作系統將IP地址返回給瀏覽器，同時也將IP緩存

7. 瀏覽器得到域名對應的IP位置

DNS解析是很耗時的，因此如果解析域名過多，會讓加載變得過慢，可以考慮dns-prefetch優化

### TCP
TCP 協議：三次握手的過程採用 TCP 協議，其可以保證信息傳輸的可靠性，三次握手過程中，若一方收不到確認信號，協議會要求重新發送信號。

### 建立連接--三次揮手
1. 主機向服務器發送一個建立連接的請求   

2. 服務器接到請求後發送同意連接的信號

3. 主機接到同意連接的信號，再次發送確認信號，連接建立完成，數據傳輸開始

### 斷開連接--四次揮手
1. 主機向服務器發送一個斷開連接的請求

2. 服務器接到請求後，發送確認收到的信號

3. 服務器向主機發送斷開通知

4. 主機接到斷開通知後斷開連接並反饋一個確認信號，服務器收到信號以後斷開連接

### 五層因特網協議
>1. 應用層(dns,http) DNS解析成IP並發送http請求
>2. 傳輸層(tcp,udp) 建立tcp連接（三次握手）
>3. 網絡層(IP,ARP) IP尋址
>4. 數據鏈陸層(PPP) 封裝
>5. 物理層(利用物理介質傳輸比特流)物理傳輸（傳輸的時後通過雙膠線，電磁波等各種介質）

### 後台和前台的HTTP交互
前後端交互時，http報文作為信息的載體，http是很重要的內容

### HTTP報文結構
> 通用頭部
>
> 請求/響應頭部
>
> 請求/響應體


#### 通用頭部
> **Request Url:** 請求的web服務器地址
>
> **Request Method:** 請求方式（Get、POST、OPTIONS、PUT、HEAD、DELETE、CONNECT、TRACE）
>
> **Status Code:** 請求的返回狀態碼，如200代表成功
>
> **Remote Address:** 請求的遠程服務器地址（會轉為IP）

#### 狀態碼
> 200——表明該請求被成功完成，所請求的資源發送回客户端
>
> 304——自從上次請求後，請求的網頁未修改過，請客户端使用本地缓存
>
> 400——客户端請求有錯（譬如可以是安全模塊攔截）
>
> 401——請求未經授權
>
> 403——禁止訪問（譬如可以是未登陸時禁止）
>
> 404——資源未找到
>
> 500——服務器内部錯誤
>
> 503——服務不可用
>
> ...

### HTTP的緩存
前後端的http交互中，使用緩存能提升效率，基本上對性能有要求的前端项目都是必用緩存的

缓存可以簡單的劃分成兩種類型：`強緩存（200 from cache）` 與`協商緩存（304）`

强缓存（200 from cache）時，瀏覽器如果判断本地缓存未過期，就直接使用，無需發起http request

協商缓存（304）時，瀏覽器會向服務端發起http請求，然後服務端告訴瀏覽器文件未改變，讓瀏覽器使用本地缓存

對於協商缓存，使用`Ctrl + F5`强制刷新可以使缓存無效，但是對於强缓存，在未過期時，必須更新資源路徑才能發起新的請求（更改了路徑相當於是另一個資源了，這也是前端工程化中常用到的技巧）

**如何區別強緩存及協商緩存？**

看緩存頭部。

屬於強緩存控制的：

>（http1.1）Cache-Control/Max-Age
>
>（http1.0）Pragma/Expires
>
> 注意：Max-Age不是一個頭部，它是Cache-Control頭部的值

屬於協商緩存控制的：

> (http1.1）If-None-Match/E-tag  
>
>（http1.0）If-Modified-Since/Last-Modified

## 瀏覽器接收到 http 數據包後的解析流程
1. 解析HTML，構建 DOM Tree

2. 解析CSS，生成CSS Rule Tree

3. 合併DOM Tree和CSS Rule Tree，生成 Render Tree

4. 布局Render Tree（Layout/reflow），負責各元素尺寸、位置的計算

5. 繪製Render Tree（paint），繪製頁面像素信息

6. 瀏覽器會將各層的信息發送给GPU，GPU會將各層合成（composite），顯示在屏幕上

渲染完畢後，JS引擎開始執行`load`事件

![渲染過程](/img/in-post/dom.jpg)





## 參考資料

* [从输入URL到页面加载的过程？如何由一道题完善自己的前端知识体系](https://zhuanlan.zhihu.com/p/34453198)
* [What happens when...](https://github.com/alex/what-happens-when)
* [浅谈浏览器多进程与JS线程](https://segmentfault.com/a/1190000013083967)

