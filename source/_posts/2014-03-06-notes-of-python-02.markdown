---
layout: post
title: "Notes of Python 02"
date: 2014-03-06 17:07:25 +0800
comments: true
categories: Python
---
1. 不同算法實現Fibonacci數列  
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
```  
3) 尾遞歸(SICP)：  
``` python
def fib(n):
 def fib_iter(n,x,y):
  if n==0 : return x
  else : return fib_iter(n-1,y,x+y)
 return fib_iter(n,0,1)
 ```  
[Origin](http://www.cnblogs.com/figure9/archive/2010/08/30/1812927.html)  
</br>
2. 九九乘法表  
``` python
for i in range(1,10):
　　for j in range(1,i+1):
　　　　print(" %d*%d=%d" % (j,i,i*j)),
　　print '\n'
```  
</br>
3. 求素數  
``` python
lis = []    #装非素数的容器
for obj in range(1,10):
    if obj>2:
        for x in range(2,obj):
            if obj % x == 0:
                lis.append(obj)
lis = list(set(lis))    #去重
sushu = [obj for obj in range(1,10) if obj not in lis]  #去包含
print sushu
```