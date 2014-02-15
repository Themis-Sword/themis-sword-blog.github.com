---
layout: post
title: "Python列表刪除重複元素的三種方法及效率分析(轉)"
date: 2014-02-15 17:53:01 +0800
comments: true
categories: 
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
```    
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
但是，set()會破壞排列順序，如果要保留排序，list(set(lists))可改為sorted(set(lists),key=lists.index)