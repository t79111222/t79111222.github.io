---
layout     : post
title      : "Android Library 發佈到JCenter"
subtitle   : "讓別人在gradle添加一行指令就引用你的Library"
date       : 2017-03-23 13:36:23 +0800
author     : "TIna"
tags       : Android JCenter
comments   : true
---

想必很多Android開發者像我一樣，常常上[GitHub](https://github.com/)尋找適合自己APP的Library，往往我們只要在module資料夾(一般開新專案的app資料夾)下的build.gradle，添加一行：

```groovy
compile 'com.squareup.retrofit2:retrofit:2.2.0'
```
就能快速地引用別人的Library，**本文章將會教大家如何產生自己的**``compile 'com.xxx.xxx:xxx:1.0.0'``，開放給大家使用你的Library，本文章以[此專案](https://github.com/t79111222/TagPicture)為範例。

## JFrog Bintray前置作業

1. 註冊JFrog Bintray  
前往[JFrog Bintray](https://bintray.com/)註冊你的帳號。(**注意!!!** 有個人/組織差別的帳號，組織的要付費才能夠發布到JCenter，就算你在組織帳號下開public的專案也一樣無法發布，TIna第一次就是註冊到組織帳號，砍帳號重來新來過才成功ＱＱ)
[![jfrog_new_account](https://t79111222.github.io/images/2017/03/jfrog_new_account.jpg)](https://t79111222.github.io/images/2017/03/jfrog_new_account.jpg)

2. 建立一個Repository  
點選『Add New Repository』，Name我填入『maven』，Type一定要選擇『**Maven**』，Default Licenses我選填了『Apache-2.0』）(也可不填)，Description可不填。  
(一個Repository可以丟很多專案上去，因此Name我沒有填入專案名稱)  
[![add_new_repository](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/add_new_repository.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/add_new_repository.png?raw=true)

## Android Project設定
一般你的Library專案目錄可能長這樣↓↓↓，有兩個Module，一個example，一個library。
[![project_folder](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/project_folder.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/project_folder.png?raw=true)

1. 設定專案的build.gradle
添加下面設定至dependencies，[看範例內容](https://github.com/t79111222/TagPicture/blob/master/build.gradle)。
```groovy
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.4.1'
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7'
```

2. 設定Library的build.gradle  
設定好 **versionCode** 和 **versionName** ，並且添加下面設定，[看範例內容](https://github.com/t79111222/TagPicture/blob/master/library/build.gradle)。  
```gradle=
apply from: "bintrayUpload.gradle"
```  

3. 設定bintrayUpload.gradle  
於你Library資料夾下新增一個檔案，檔名為『bintrayUpload.gradle』，[複製此路徑的內容進去](https://github.com/t79111222/TagPicture/blob/master/library/bintrayUpload.gradle)，完全不用修改，除非你licenses不用Apache-2.0。

4. 設定Library的project.properties  
於你Library資料夾下新增一個檔案，檔名為『project.properties』，此檔案為bintrayUpload.gradle的設定檔，
[看範例內容](https://github.com/t79111222/TagPicture/blob/master/library/project.properties)：
```java=
    #project
    project.repositoryName=   
    project.name=  
    project.groupId=
    project.artifactId=  
    project.packaging=aar  
    project.siteUrl=
    project.gitUrl=

    #javadoc  
    javadoc.name=  
```   

    參數說明
      + project.repositoryName：JFrog Bintray前置作業第二步驟的Repository name (也就是『maven』)
      + project.name：你的專案名稱(以上方目錄截圖為例，就是最外層資料夾名稱『TagPicture』)
      + project.groupId：通常是你的Library的package(看你Library的AndroidManifest.xml裡的package內容是什麼，[看範例的AndroidManifest](https://github.com/t79111222/TagPicture/blob/master/library/src/main/AndroidManifest.xml)，也就是填入『com.yiting.tagpicture』)
      + project.artifactId：填入你的Library名稱(以上方目錄截圖為例，就是填入『library』)
      + project.packaging：Android Library固定填入『aar』
      + project.siteUrl：你的Library官方網站，沒有就填入你GitHub的路徑(如範例『https://github.com/t79111222/TagPicture  』)
      + project.gitUrl：git的url(如範例『https://github.com/t79111222/TagPicture  』)
      + javadoc.name：產生javadoc時的名稱，通常和project.name一樣就可以了

5. 設定Library的local.properties  
於你Library資料夾下新增『local.properties』，這個檔案一樣是bintrayUpload.gradle的設定檔，但是放置的是比較私密的內容，因此**記得在你的gitignore添加忽略喔！**
```java=
#bintray
bintray.user=
bintray.apiKey=
#developer
developer.id=
developer.name=
developer.email=
```  
參數說明  

  + bintray.user：JFrog Bintray帳號，也就是最右上的帳號
[![bintray.user](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/bintray_user.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/bintray_user.png?raw=true)  

  + bintray.apiKey：依下圖指示取得(Edit Profile > API Key)  
[![get_api_key](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/get_api_key.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/get_api_key.png?raw=true)  

  + developer.id：你的暱稱
  + developer.name：你的名字
  + developer.email：你的Email

## 下指令將你的專案推上去
用Android Studio打開這個Library專案，直接使用內建Terminal下指令(好處是不用切換目錄)，以下指令為mac的，windows去掉『./』就好囉。
+ 先這個指令，用來下載依賴的Jar，有跑出『BUILD SUCCESSFUL』表示成功
```java=
./gradlew install
```
+ 再下這個指令，用來將你的專案推到JFrog Bintray，有跑出『BUILD SUCCESSFUL』表示成功
```java=
./gradlew bintrayUpload
```

## 提交你的專案到JCenter
進入到『JFrog Bintray』點選你建立的Repository，進入後會看到你的專案，再點進去裡面會有個『Add to JCenter』點選進入，於輸入框填入你的Library訊息，其實可以隨便打，我是輸入『This library for android developer.』，然後耐心等待審核通過，可能要5.6小時以上，如果通過會寄email通知。
[![add_to_jcenter](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/add_to_jcenter.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/add_to_jcenter.png?raw=true)

## 你的Gradle
在這裡可以找到你的gradle
[![your_gradle](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/your_gradle.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/your_gradle.png?raw=true)
