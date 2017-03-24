---
layout     : post
title      : "看不到Android原碼!!"
subtitle   : "點快捷鍵要看Android原生物件的程式碼卻看不到內容"
date       : 2017-03-24 14:32:24 +0800
author     : "TIna"
tags       : Android
comments   : true
---

TIna很常在開發的時候用『command + 游標選擇物件方法』來去看看這個方法或這個calss寫了什麼東西，今天TIna就發生我只是想看看RelativeLayout裡的某個方法結果就出現：
```
Decompiled .class file, bytecode version:51.0(Java 7)
Sources for 'Android API 23 Platform' not found.
```
[![android_api_not_found](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/android_api_not_found.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/android_api_not_found.png?raw=true)

裡面完全沒有function的詳細內容，就算點了Download後，再怎麼點Refresh也沒反應

### 解決方法

  1. 進入到Android SDK設定，點選 Edit  
>Windows： File -> Settings -> Appearance & Behavior -> System Settings -> Android SDK.  
>
> Mac： Android Studio -> Preferences -> Appearance & Behavior -> System Settings -> Android SDK. )

+ [![android_sdk_edit](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/android_sdk_edit.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/android_sdk_edit.png?raw=true)

2. 點選 Edit 後，進入到 『SDK Components Setup』，什麼都不動直接一直點『Next』到底，然後應該會自動重新整理，你的程式碼就出現了!  
[![sdk_setup_next](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sdk_setup_next.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sdk_setup_next.png?raw=true)
