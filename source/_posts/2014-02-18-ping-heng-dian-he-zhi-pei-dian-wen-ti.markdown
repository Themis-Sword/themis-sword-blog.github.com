---
layout: post
title: "平衡點問題和支配點問題(轉)"
date: 2014-02-18 21:46:13 +0800
comments: true
categories: python
keywords: 平衡點，支配點，
description: 平衡點和支配點
---

**1. 平衡點問題**  
平衡點：比如int[] numbers = {1,3,5,7,8,25,4,20}; 25前面的總和爲24，25後面的總和也是24，25這個點就是平衡點；假如一個數組中的元素，其前面的部分等於後面的部分，那麼這個點的位序就是平衡點。   
要求：返回任何一個平衡點  
使用sum函数累加所有的数。  
使用一个变量fore来累加序列的前部。直到满足条件fore<(total-number)/2;<!--more-->  
``` python  
numbers = [1,3,5,7,8,2,4,20]  
#find total  
total=sum(numbers)  
#find num  
fore=0  
for number in numbers:  
   if fore<(total-number)/2 :  
      fore+=number  
   else:  
      break  
#print answer  
if fore == (total-number)/2 :  
   print number  
else :  
   print r'not found'  
```  
算法簡單，而且是O(n)的。  
**PS** 上述解題思路爲題目只考慮序列只包括正數的情況(有唯一平衡點)，當序列中有負數的時候，平衡點不一定唯一。  
  
**2. 支配點問題**  
支配數：數組中某個元素出現的次數大於數組總數的一半時就成爲支配數，其所在位序成為支配點；比如int[] a = {3,3,1,2,3};3爲支配數，0，1，4分别爲支配點。  
要求：返回任何一個支配點  
《編程之美》中有答案，就是尋找水王那篇。  
具體方法是：將序列排序，取中位數——注意，如果一個數出現次數大於整體的一半，那麼排序之後支配數一定在中間，然後驗證是否正確。  
``` python  
numbers = [1,3,4,3,3]  
#calculate  
numbers.sort()  
lens=len(numbers)  
candidate=numbers[lens/2]  
#validate  
N=0  
for number in numbers:  
    if number==candidate:  
       N+=1  
#print answer  
if (N>=lens/2):  
     print numbers[lens/2]  
else :  
   print 'not found'  
```  
[Origin](http://hi.baidu.com/ruclin/item/f2706f26b1d2db140975086b)
[Reference](http://www.iteye.com/topic/600079)