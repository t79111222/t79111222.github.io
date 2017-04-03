---
layout     : post
title      : "取得手機內部DB"
subtitle   : "從Root或沒Root的手機取得DB，來協助debug"
date       : 2017-04-03 17:23:41 +0800
author     : "TIna"
tags       : Android
comments   : true
---

開發的時候，有時候遇到SQLite的錯誤，像是查詢條件錯誤呀，或是好像存了正確資料，但是取資料出來顯示，怎麼會是錯的呢？這時候你可能會用Log印出來資料內容，但是如果能夠快速的取的整個DB，並視覺化的快速瀏覽，就可以很快的解決問題了。

## DB檔放哪裡？
> /data/data/your_package_name/databases/your_db_name.db

## 已Root的設備
如果你是已經root過的設備，要取得.db檔其實非常的簡單，只要透過Android Device Monitor裡面的File Explorer就能快速地拿到.db檔，並輸出到你的電腦中

> Android Studio 工具列 > Tools > Android > Android Device Monitor > File Explorer

如下圖，找到你的db檔放的位置，由於TIna手邊沒有Root設備，所以只能提供到data路徑的截圖
[![file_explorer](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/04/file_explorer.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/04/file_explorer.png?raw=true)

## 沒有Root的設備
沒有root的設備沒辦法使用File Explorer，因為沒有權限，所以從File Explorer只能看到data資料夾。  
所以只能下指令進入設備將db檔複製到，手機的sdcard中，在下載到電腦中。

連上手機後，在USB偵錯模式，直接使用Android Studio的Terminal下指令
>huangyitingde-MacBook-Pro:DrinkOrder Yi$ **adb shell** _//進入你的手機設備中_    
>shell@htc_a16ul:/ $ **run-as com.your.packageName** _//進入你的app資料夾_  
>shell@htc_a16ul:/data/data/com.your.packageName $ **ls** _//看看有什麼資料_  
>cache  
>code_cache  
>databases  
>shared_prefs  
>shell@htc_a16ul:/data/data/com.your.packageName $ **cd databases** _//進入databases資料夾_  
>shell@htc_a16ul:/data/data/com.your.packageName/databases $ **ls**  
>yourDbName.db  
>yourDbName.db-journal  
>shell@htc_a16ul:/data/data/com.your.packageName/databases $ **cat yourDbName.db > /mnt/sdcard/yourDbName.db** _//複製一份db至sdcard裡_   
>shell@htc_a16ul:/data/data/com.your.packageName/databases $ **exit** _//退出你的app資料夾_  
>shell@htc_a16ul:/ $ **exit** _//退出你的手機設備_  
>huangyitingde-MacBook-Pro:DrinkOrder Yi$ **adb pull /sdcard/yourDbName.db /your/computer/path/yourDbName.db** _//將db複製到電腦裡_   

## 看DB內容
找個看SQLite的DB Browser，看看你的DB內容，我使用的是[DB Browser for SQLite](http://sqlitebrowser.org/)，他有支援Windows & Mac ，雖然畫面很陽春，但我覺得很好用。
