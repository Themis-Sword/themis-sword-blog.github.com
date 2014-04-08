---
layout: post
title: "刪除列表重複元素的三種方法及效率分析(轉)"
date: 2014-02-15 17:53:01 +0800
comments: true
categories: python
keywords: python, 列表，重複，方法，效率
description: 刪除列表重複元素的三種方法及效率分析
---
####方法一：  
使用列表對象的sort()方法對列表進行排序，從最後一個元素開始循環迭代列表，判斷相鄰的兩個元素是否相同。  
``` python
def methodONE(list):
    list.sort()
    lenList = len(list)
    lastItem = list[lenList-1]
    for i in range(lenList-2,-1,-1):
     if list[i] == lastItem:
      list.remove(list[i])
     else:
      lastItem = list[i]
    return list
```    <!--more-->
####方法二：  
定義一個臨時列表，循環迭代出的元素如果不在循環列表中，則加入，最後返回列表。  
``` python
def methodTWO(list):
    templist = []
    for i in list:
     if not i in temp list:
      templist.addend(i)
    return templist
```  
####方法三：
``` python
list = [20,12,34,12,24,34,55,27]
print list(set(lists))
```  
####分析：
1. 方法一相對方法二來說，有更多的額外操作如：排序、賦值。因爲在Python中，變量是不可變的，每迭代出一个元素比較不相等後的操作都是新建立一个局部變量、賦值並丟棄原變量，這需要消耗更多的内存!同時因爲排序操作，破壞了相對位置。  
2. 方法二建立一个臨時列表進行操作，而列表是可變的，每次追加元素都只是在原列表上增加一个索引及值,因而相對方法一來說效率會更高!  
3. 方法三无疑是效率最好的(無論是代碼的簡潔還是運行效率)。set()是內置的數據類型“集合類型”，它是無序的且值是唯一的！所以set()執行的结果就是轉為集合且直接去除了重複的元素，list()則將集合轉回列表類型。  
但是，set()會破壞排列順序，如果要保留排序，list(set(lists))可改為sorted(set(lists), key=lists.index)  
  
在itertools有个强大的函数groupby可以很快捷的实现:  
``` python
from itertools import *

a = [1, 4, 5, 4, 9, 1, 2, 3, 4, 5, 11]
a.sort()
b = [k for k, g in groupby(a)]
print b
```  
  
[Origin](https://github.com/bluescorpio/mydocuments/blob/master/Todo/%E5%88%A0%E9%99%A4%E5%88%97%E8%A1%A8%E4%B8%AD%E9%87%8D%E5%A4%8D%E7%9A%84%E5%85%83%E7%B4%A0%E5%8F%8A%E6%95%88%E7%8E%87%E5%88%86%E6%9E%90.txt)