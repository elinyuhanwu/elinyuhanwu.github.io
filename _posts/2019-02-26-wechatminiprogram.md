---
layout:     post
title:      "微信小程序"
subtitle:   "\"Some notes from my practice of wechat mini-program\""
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

## 小程序基本架構：MINA框架
View 視圖層：渲染頁面結構。WXML、WXSS

App Service 邏輯層：邏輯處理、數據請求、接口調用

## 開發人員看小程序
**頁面管理：** 由框架管理整個小程序的頁面路由，讓頁面能無縫切換，並給頁面完整的生命週期，開發人員只需要專注於將頁面的數據、方法、生命週期函數註冊進框架中，其他複雜操作會由框架完成。

**wechat提供的豐富API：** 框架提供了豐富的原生API，可以方便的調用微信提供的功能，例如：獲取用戶信息、本地存儲、支付功能等等。

## 小程序配置

**app.json：** 對小程序進行全局配置，決定頁面路徑，窗口表現，設置網絡超時時間，設置多tab......。

```javascript
{
  "pages": [
    "pages/index/index",
    "pages/logs/index"
  ],  
"window": {
    "navigationBarTitleText": "Demo"
  },
  "tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页"
    }, {
      "pagePath": "pages/logs/logs",
      "text": "日志"
    }]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": true
}
```

**app.json配置列表**

|屬性           | 類型       | 必填 | 描述 |
|---           | ---        | --- | --- | 
|pages         |String Array| 是   | 設置頁面路徑|
|window        | Object     | 否   | 設置默認頁面的窗口表現|
|tabBar        |Object      |否    |設置底部tab的表現|
|networkTimeout|Object      |否    |設置網絡超時時間|
|debug         |Boolean     |否    |設置是否開啟debug模式|


> 小程序篇就先寫到這邊，開始寫的時間是入職前，到今天再度更新，已經開始有了些實戰經驗，相信未來再寫關於小程序的文章會更有內容一些。




