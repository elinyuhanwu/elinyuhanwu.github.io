---
layout:     post
title:      "Cross Browser Testing"
subtitle:   " \"Cross browser testing\""
date:       2019-01-15 12:00:00
author:     "Han"
header-img: "img/post-bg-browser.jpeg"
catalog: true
tags:
    - 筆記
    - Web
    - 前端
---



## Work Flow for Cross Browser Testing
> Initial planning > Development > Testing/discovery > Fixes/iteration 

### Initial Planning
Explored the target audience — what browsers, devices, etc.

Define target testing platform.

### Development
* Get all the functionality working as closely as possible in all target browsers.
    * different code path
    * Polyfill
    * library
* Accept that some things aren't going to work the same on all browsers, and provide different (acceptable) solutions in browsers that don't support the full functionality. 
* Accept that your site just isn't going to work in some older browsers, and move on. 
**Don't leave all the testing till the end!**

### Testing / Discovery
1. Test it in a couple of stable browsers on your system, like Firefox, Safari, Chrome, or IE/Edge. And test the latest change on all the modern desktop browsers you can include: Firefox, Chrome, Opera, IE, Edge, and Safari on desktop (Mac, Windows, and Linux, ideally).
2. Do some low fi accessibility testing, such as trying to use your site with only the keyboard, or using your site via a screen reader to see if it is navigable.
3. Test on a mobile platform, such as Android or iOS.
4. You can set up your own testing automation system ([Selenium](https://www.seleniumhq.org) being the popular app of choice) that could for example load your site in a number of different browsers, and: 

* take a screenshot of each result, allowing you to see if a layout is consistent across the different browsers.

You can also go further than this, if wished. There are commercial tools available such as [Sauce Labs](https://saucelabs.com), [Browser Stack](https://www.browserstack.com), and [LambdaTest](https://www.lambdatest.com) that do this kind of thing for you, without you having to worry about the setup, if you wish to invest some money in your testing. It is also possible to set up an environment that automatically runs tests for you, and then only lets you check in your changes to the central code repository if the tests still pass.

#### Testing on prerelease browsers

This is especially prevalent if you are using very new technologies in your site, and you want to test against the latest implementations, or if you are coming across a bug in the latest release version of a browser, and you want to see if the browser's developers have fixed the bug in a newer version.














## 參考資料
[MDN web Docs](https://developer.mozilla.org/zh-TW/docs/Learn/Tools_and_testing/Cross_browser_testing)

[caniuse](https://caniuse.com)