---
layout:     post
title:      "微信小程序"
subtitle:   "Take some notes from my practice of wechat mini-program"
date:       2019-02-26 12:00:00
author:     "Han"
header-img: "img/post-bg-miniprogram.jpeg"
catalog: true
tags:
    - 筆記
    - 小程序
    - 前端
---

> 在前端領域必須時刻不斷學習，來到上海，理所當然要學習的就是Wechat Mini Program

**什麼是小程序？**

不需要下載安裝即可以使用的應用，實現了觸手可及的夢想，用戶只要掃一掃或是搜一搜就可以打開應用程式。同時也體現了用完就走的理念，用戶不用關心是否安裝太多應用的問題。

**小程序與APP的區別**
* 小程序只能在微信的進程中運行
* 只要開發一個小程序便可以在 Android / IOS 等不同的設備上運行
* 不能繞過 wechat 調用 API
* 不能最離線存儲，只能存儲到 Local Storage
* 通過 Webview 進行渲染

**小程序與H5的區別**

運行環境不同，無法使用window對象和document對象

## 小程序基本架構
View 視圖層：渲染頁面結構

App Service 邏輯層：邏輯處理、數據請求、接口調用