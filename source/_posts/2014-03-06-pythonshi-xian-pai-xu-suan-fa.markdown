---
layout: post
title: "排序算法"
date: 2014-03-06 15:53:21 +0800
comments: true
categories: python
keywords: 冒泡，排序，插入，選擇，快速，函數
description: 幾種排序算法
---
**1. 冒泡排序**  
``` python   
def bubble(x,n):  
\冒泡排序，x是列表，n是列表長度  
for i in range(n):  
 for j in range(n-1):  
  if x[j]>x[j+1]:  
   t = x[j]  
   x[j] = x[j+1]  
   x[j+1] = t  
return x                  
print bubble([1,10,2,5,41,25,3,48], 8)    
```  
</br><!--more-->
**2. 插入排序**  
``` python  
def insert(x,n):
     i = 1
     while i<n-1:
         key = x[i]
         j = i-1
         while j>=0 and key<x[j]:
             x[j+1]= x[j]
             j -= 1
         x[j+1] = key
         i += 1
     return x
print insert([1,10,2,5,41,25,3,48],8)
```  
</br>
**3. 選擇排序**  
``` python
def select(x,n):
    for i in range(n-1):
        key = i
        for j in range(i+1,n):
            if x[j] < x[key]:
                 key = j
        if key!=i:
            t = x[i]
            x[i] = x[key]
            x[key] = t
    return x
print select([1,10,2,5,41,25,3,48],8)
```  
</br> 
**4. 快速排序**
``` python
def partition(x,low,high):
    key = x[low]
    while low<high:
        while low<high and x[high]>=key:
            high -= 1
        if low < high:
            x[low]= x[high]
            low += 1
        while low <high and x[low]<=key:
            low += 1
        if low < high:
            x[high] = x[low]
            high -= 1
    x[low] = key
    return low
def quick(x,low,high):
    if low < high:
       p = partition(x,low,high)
       quick(x,low,p-1)
       quick(x,p+1,high)
    return x
```  
</br>
**5. 利用函數排序**  
1) cmp()  
Compare the two objects x and y and return an integer according to the outcome. The return value is negative if x < y, zero if x == y and strictly positive if x > y.  
2) reversed()  
3) sort()  
4) sorted()  