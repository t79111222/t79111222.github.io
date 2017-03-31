---
layout     : post
title      : "ORM Sugar的查詢條件遇到的錯誤"
subtitle   : "Ｗhy?! 查詢條件用其他欄位查詢卻發生錯誤"
date       : 2017-03-31 15:54:41 +0800
author     : "TIna"
tags       : Android ORM-Sugar
comments   : true
---

一開始下的查詢條件都好好的，拿得到內容
```java=
Drink.listAll( getTableClass(), "sorting asc")
```
直到...我下這樣的查詢出現錯誤
```java=
DrinkSugar.find(getTableClass(), “drinkId = ? and sugarId = ?", drinkId, sugarId);
```
錯誤內容是：

>android.database.sqlite.SQLiteException: no such column: drinkId  

比對Table看起來沒有錯呀！！！為啥他說沒有這個欄位！

```java=
public class DrinkSugar extends SugarRecord {
    public long drinkId;
    public long sugarId;

    public DrinkSugar(){

    }

    public DrinkSugar(long drinkId, long sugarId){
        this.drinkId = drinkId;
        this.sugarId = sugarId;
    }
}
```
## 找原因-直接看DB內容
透過工具看，由ORM Sugar建立table時，於Sql中建立了一張名為『DRINK_SUGAR』的表單而裡面的欄位為ID,DRINK_ID,SUGAR_ID，我大寫的部分**前面被Sugar加底線了**  
[![sugar_table1](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sugar_table1.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sugar_table1.png?raw=true)

依照查到的table，正確的查詢條件是
```groovy=
DrinkSugar.find(getTableClass(), “drink_id = ? and sugar_id = ?", drinkId, sugarId);
```  

## 想要名稱一致卻不行
這時候我就想，不如果把DrinkSugar改成這樣，兩個者就一致了
```java=
public class DrinkSugar extends SugarRecord {
    public long drink_id;
    public long sugar_id;

    public DrinkSugar(){

    }

    public DrinkSugar(long drinkId, long sugarId){
        this.drink_id = drinkId;
        this.sugar_id = sugarId;
    }
}
```
結果還是Error...table內容變這樣，**底線被去掉了**  
[![sugar-table2](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sugar-table2.png?raw=true)](https://github.com/t79111222/t79111222.github.io/blob/master/images/2017/03/sugar-table2.png?raw=true)

## 結論
想要一致性和可讀性兼顧根本不可能ＱＱ  
所以要我選擇維持用```drinkId```，只是在查詢時記得在大寫字母前加個底線  

## 下篇預告
>如何從未Root的手機取得DB，來協助debug
