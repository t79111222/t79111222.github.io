---
layout     : post
title      : "Android Library 發佈到JCenter"
subtitle   : "讓別人在gradle添加一行指令就引用你的Library"
date       : 2017-03-23 13:36:23 +0800
author     : "TIna"
tags       : Androd JCenter
comments   : true
---

想必很多Android開發者像我一樣，常常上[Github](https://github.com/)尋找適合自己APP的library，往往我們只要在module資料夾(一般開新專案的app資料夾)下的build.gradle，添加一行：

```groovy
compile 'com.squareup.retrofit2:retrofit:2.2.0'
```
就能快速地引用別人的library，
**本文章將會教大家如何產生自己的**``compile 'com.xxx.xxx:xxx:1.0.0'``，開放給大家使用你的library。

## 1.註冊JFrog
前往[JFrog Bintray](https://bintray.com/)註冊你的帳號。(**注意!!!** 有個人/組織差別的帳號，組織的要付費才能夠發布到JCenter，就算你在組織帳號下開public的專案也一樣無法發布，TIna第一次就是註冊到組織帳號，砍帳號重來新來過才成功ＱＱ)

![jfrog_new_account](../images/2017/03/jfrog_new_account.jpg)
