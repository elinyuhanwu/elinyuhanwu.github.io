---
layout: post
title: "從輸入URL到頁面加載中間發生了什麼？"
subtitle: "What happened from typing URL to page loading in the browser?"
author: "Han"
header-img: "/img/post-bg-frontendURL.jpeg"
header-mask: 0.3
tags:
  - 前端
  - 生活
---
> 最近在準備面試新的前端工作，看到了這個題目，就當作Blog中第一篇前端文章，以作為自己基礎知識梳理習慣的開始

在前端的世界中，每天都有新的框架、新的設計、新的想法，但沒有了基礎知識的全盤瞭解，很容易在運用新技術時無法順利上手，遇到Bug卻不得解，我深知自己並非計算機相關科系專業，遇到這種在程序之前的題目，更需要花時間學習了解，關於這個題目的文章網路上有很多，這篇文章主要是我自己的一些學習筆記、重點整理，也先為之後準備寫的PWA鋪程。


>**主要回答**
1. 從瀏覽器接收 url 到開啟網絡請求線程 ( 展開瀏覽器的機制以及進程與線程之間的關係 )
2. 開啟網絡線程到發出一個完整的 http 請求 ( dns查询, tcp/ip 請求, 五層Internet協議等 )
3. 從服務器接收到請求到對應後台接收到請求 ( 負載均衡,安全攔截以及後台內部處理等等 )
4. 後台與前台的 http 交互 ( http header, 響應碼, 報文結構, cookie等知識, 靜態資源的cookie優化以及編碼解碼, 如gzip壓縮等 )
5. 緩存問題, http 的緩存 ( http緩存header, etag, catch-control等 )
6. 瀏覽器接收到 http 數據包後的解析流程 ( 解析html -> 語法分析後解析成dom樹, 解析css生成css規則樹, 合併成render樹, 然後layout, painting渲染, 復合塗層的合成, GPU繪製, 外鍊資源的處理, loaded和domContentLoaded等 )
7. CSS的可視化格式模型 ( 元素的渲染規則, 如包含塊, 控制框, BFC, IFC等概念 )
8. JS引擎解析過程 ( JS的解釋階段, 預處理階段, 執行階段生成執行上下文, VO, 作用域鍊, 回收機制等等 )
9. 其它 ( 跨域, web安全, hybrid模式...... )


