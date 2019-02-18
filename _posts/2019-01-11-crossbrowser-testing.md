---
layout:     post
title:      "Cross Browser Testing"
subtitle:   " \"testing web projects across different browsers\""
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

## Defensive programming
A common approach is to have multiple grades of support level, first support chart for example: 
1. A grade: Common/modern browsers — Known to be capable. Test thoroughly and provide full support.

2. B grade: Older/less capable browsers — known not to be capable. Test, and provide a more basic experience that gives full access to core information and services.

3. C grade: Rare/unknown browsers — don't test, but assume they are capable. Serve the full site, which should work, at least with the fallbacks provided by our defensive coding.

In China final support chart will be like: 

1. A grade: Chrome and Firefox for Windows/Mac, Safari for Mac, Edge and IE for Windows (last two versions of each), iOS Safari for iPhone/iPad, Android stock browser (last two versions) on phone/tablet, Chrome and Firefox for Android (last two versions) on phone tablet. Accessibility passing common tests.

2. B grade: IE 8 and 9 for Windows, Opera Mini.

3. C grade: Opera, other niche modern browsers.

## Physical Devices
* A Mac, with the browsers installed that you need to test — this can include Firefox, Chrome, Opera, and Safari.

* A Windows PC, with the browsers installed that you need to test — this can include Edge (or IE), Chrome, Firefox, and Opera.

* A higher spec Android phone and tablet with browser installed that you need to test — this can include Chrome, Firefox, and Opera Mini for Android, as well as the original Android stock browser.

* A higher spec iOS phone and tablet with the browsers installed that you need to test — this can include iOS Safari, and Chrome, Firefox, and Opera Mini for iOS.

## HTML and CSS
**HTML**
A good strategy is to validate your code regularly. One service that can do this is the [W3C Markup Validation Service](https://validator.w3.org/), which allows you to point to your code, and returns a list of errors.
**CSS**
1. Linters: linter plugins => [atom](https://atom.io/)

2. Browser Developer Tools









## 參考資料
[MDN web Docs](https://developer.mozilla.org/zh-TW/docs/Learn/Tools_and_testing/Cross_browser_testing)

[caniuse](https://caniuse.com)