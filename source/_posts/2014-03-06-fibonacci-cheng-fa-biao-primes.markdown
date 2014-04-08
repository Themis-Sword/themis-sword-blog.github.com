---
layout: post
title: "Fibonacci序列, 乘法表與求素數的算法"
date: 2014-03-06 17:07:25 +0800
comments: true
categories: python
keywords: python, 算法, fibonacci, 乘法表, 素數
description: 算法
---
**1. 不同算法實現Fibonacci數列**  
1) 遞歸：  
``` python
fib=lambda n:1 if n<=2 else fib(n-1)+fib(n-2)
```  
2) 迭代：  
``` python
def fib(n):
 x,y=0,1
 while(n):
  x,y,n=y,x+y,n-1
 return x
```<!--more-->   
3) 尾遞歸(SICP)：  
``` python
def fib(n):
 def fib_iter(n,x,y):
  if n==0 : return x
  else : return fib_iter(n-1,y,x+y)
 return fib_iter(n,0,1)
```  
[Origin](http://www.cnblogs.com/figure9/archive/2010/08/30/1812927.html) 
  
4-1) yield生成器(尾數不大於1000的序列):  
``` python
def fibonacci():
    a, b = 0, 1
    while True:
    	yield a
        a,b = b,a+b

for i in fibonacci():
    if i > 1000:
        break
    print i
```  
4-2) yield生成器(生成10個元素的序列):  
``` python
def fibonacci(max):
    a, b, n = 0, 1, 0
    while n < max:
        yield a
        a, b = b, a + b
        n += 1

for i in fibonacci(10):
    print i
```  
  
**2. 九九乘法表**  
``` python
for i in range(1,10):
　　for j in range(1,i+1):
　　　　print(" %d*%d=%d" % (j,i,i*j)),
　　print '\n'
```  
  
**3. 求素數**  
01)  
``` python
lis = []
for obj in range(1,10):
    if obj>2:
        for x in range(2,obj):
            if obj % x == 0:
                lis.append(obj)
lis = list(set(lis))
primes = [obj for obj in range(1,10) if obj not in lis]
print primes
```  
02)  利用python的數學函數  
``` python
import math  

def isPrime(n):  
    if n <= 1:  
    return False 
    for i in range(2, int(math.sqrt(n)) + 1):  
    if n % i == 0:  
        return False 
    return True 
```  

03) 單行程序掃描素數  
``` python
from math import sqrt  
N = 100 
[ p for p in   range(2, N) if 0 not in [ p% d for d in range(2, int(sqrt(p))+1)] ]  
```   
04) 運用python的itertools模塊  
``` python
from itertools import count  
def isPrime(n):  
    if n <= 1:  
        return False 
    for i in count(2):  
        if i * i > n:  
            return True 
        if n % i == 0:  
            return False
```  
05)  
``` python
def isPrime(n):  
    if n <= 1:  
        return False 
    i = 2 
    while i*i <= n:  
        if n % i == 0:  
            return False 
        i += 1 
    return True 
```
  
**4. 從Python求素數到list與dict的比較(轉)**  
**原理:**  
素數，指在一個大於1的自然數中，除了1和此整數自身外，不能被其他自然數整除的數。在加密應用中起重要的位置，比如廣為人知的RSA算法中，就是基於大整數的因式分解難題，尋找兩個超大的素數然後相乘作為密鑰的。一個比較常見的求素數的辦法是[埃拉托斯特尼篩法(the Sieve of Eratosthenes)](http://zh.wikipedia.org/wiki/%E5%9F%83%E6%8B%89%E6%89%98%E6%96%AF%E7%89%B9%E5%B0%BC%E7%AD%9B%E6%B3%95)，說簡單一點就是畫表格，然後刪表格，如圖所示：  
{% img /images/sieve.gif %}  
從2開始依次往後面數，如果當前數字一個素數，那麽就將所有其倍數的數從表中刪除或者標記，然後最終得到所有的素數。  
  
**實現:**  
01)  
首先用dic存儲n範圍內的元素，然後依次標記，最後從頭掃描一次dic，把合適的元素放在list中返回結果  
``` python
def prime(n):
    lis = {}
    for i in xrange(2,n+1):     
        if not i in lis:
            lis[i] = 1
            k = i*2
            while k <= n:
                lis[k] = 0
                k = k+i
    ans = []    
    for i in lis:
        if lis[i] == 1:
            ans.append(i)
    return ans
```  
測試以後得到的結果是：求一千萬以內的素數用了9.68秒。  
可是我覺得使用1個dict和1個list存會很浪費空間（其實不然，一千萬以內的素數只有66萬個，相對一千萬來說可以忽略不計），所以想改進一下算法。  

02)  
這次只用一個list存數字，然後遍歷刪除
``` python
def primeC(n):
    lis = range(2,n+1)
    for i in xrange(2,n+1):
        if i in lis:
            k = i+i
            while k<=n:
                if k in lis:
                    lis.remove(k)
                k = k+i
    return lis
```  
因為無法在for中刪除list，所以使用一個xrange，然後每次判斷k是否在lis中再做刪除，結果：求十萬以內的素數一共花了186.79秒。  
  
**比較:**  
在時間上，第一個方法遠遠比第二個辦法有效率，dict是哈希實現，查詢的速度是常數級的，所以在標記合數的時候所花費的時間非常少，但是list是順序表，不知道內部是怎麽實現in 和 remove的，從時間上可以大概推測出應該是順序查找實現的，所以用如下代碼做了測試:  
``` python
import random
import time
def find(source,test):
    for each in test:
        each in source

def remove(liss,test):
    for each in test:
        if each in liss:
            liss.remove(each)


def removeDic(dicc,test):
    for each in test:
        if each in dicc:
            dicc[each] = 1

n = 100000
m = 1000
liss = range(n)
dicc = {i:0 for i in xrange(n)}
test = []
for i in xrange(m):
    test.append(random.randint(0,n))
    


s = time.clock()
find(liss,test)
print time.clock()-s


s = time.clock()
find(dicc,test)
print time.clock()-s

s = time.clock()
remove(liss,test)
print time.clock()-s

s = time.clock()
removeDic(dicc,test)
print time.clock()-s
```  
結果是:  
``` python
1.10526960281
0.00036479383832

2.11714318396
0.000374692766596
```  
可以看出，在查找方面list是遠遠不如dict的，說明list不應該是哈希查找，而remove的時間也遠遠大於哈希的賦值時間。  
不過由此判斷第二種方法是一無是處的話未免言之過早了。因為第二種方法裏面的實現是先申請了一段空間，然後再運算，而實現一的空間是動態增長的(在給哈希表賦值的那邊體現)，所以如果漲到一半發現內存不夠用的話就會報錯（測試1億數據的時候就出現了這個問題），而實現二一開始就會報錯，而不用做前面的一系列無用功。  
總之就是空間換時間，時間換空間的那些戲碼。不過挺有意思。  
  
[Origin](http://www.cnblogs.com/liga/archive/2012/08/31/2665811.html)