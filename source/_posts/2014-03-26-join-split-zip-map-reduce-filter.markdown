---
layout: post
title: "Python 常用函數之Join, Split, Zip, Map, Reduce, Filter"
date: 2014-03-26 15:34:48 +0800
comments: true
categories: python
keywords: python, join, split, zip, map, reduce, filter
description: python方法
---
***1. Join, Split***  
*A 關於Join和Split方法*  
1) 只針對字符串進行處理。split:拆分字符串、join連接字符串  
2) string.join(sep): 以string作為分割符，将sep中所有的元素(字符串表示)合併成一個新的字符串  
3) string.split(str=' ',num=string.count(str)): 以str為分隔符，切片string，如果num有指定值，則僅分隔num個子字符串  
4) 對導入os模塊進行os.path.splie()/os.path.join() 貌似是處理機制不一樣，但是功能上一樣<!--more-->  
  
*B Join*  
``` python
a='abcd'
print '.'.join(a)   
print '|'.join(['a','b','c'])　　#可以把['a','b','c']看做是 a='abcd';下面同理
print '.'.join({'a':1,'b':2,'c':3,'d':4})
```  
注意：'.'等做分隔符，將join裏的所有元素(字符串)通過分隔符連接成一個新的字符串。  
  
**os.path.join(path1[,path2[,......]])**  
``` python
\\将多个路径组合后返回，第一个绝对路径之前的参数将被忽略。 
>>> os.path.join('c:\\', 'csv', 'test.csv')
'c:\\csv\\test.csv'
>>> os.path.join('windows\temp', 'c:\\', 'csv', 'test.csv')
'c:\\csv\\test.csv'
>>> os.path.join('/home/aa','/home/aa/bb','/home/aa/bb/c')
'/home/aa/bb/c'
```
  
*C Split*  
``` python
s='a b c'
print s.split(' ')
st='hello world'
print st.split('o')
print st.split('o',1)
--------output---------
['a', 'b', 'c']
['hell', ' w', 'rld']
['hell', ' world']
```  
注意：分隔符不能為空，否則會報錯，但是可以有不含其中的分隔符：  
``` python
>>> s.split('x')
['a b c']
>>> s.split('xsdfadsf')
['a b c']
```   
  
**os.path.split()**  
os.path.split()是按照路徑將文件名和路徑分隔開，比如d:\\python\\python.ext，可分割為['d:\\python', 'python.exe']  
``` python
import os
print os.path.split('c:\\Program File\\123.doc')
print os.path.split('c:\\Program File\\')
-----------------output---------------------
('c:\\Program File', '123.doc')
('c:\\Program File', '')
```  
[Origin](http://www.cnblogs.com/BeginMan/archive/2013/03/21/2972857.html)  
  
***2. Zip***  
zip()是Python的内建函數，(與序列有關的内建函數有：sorted()、reversed()、enumerate()、zip()),其中sorted()和zip()返回一個序列(列表)對象，reversed()、enumerate()返回一個迭代器(類似序列)。  
定義：zip([seql, ...])接受一系列可迭代對象作為參數，將對象中對應的元素打包成一個個tuple（元组），然後返回由這些tuples組成的list（列表）。若傳入參數的長度不等，则返回list的長度和參數中長度最短的對象相同。  
``` python
>>> z1=[1,2,3]
>>> z2=[4,5,6]
>>> result=zip(z1,z2)
>>> result
[(1, 4), (2, 5), (3, 6)]
>>> z3=[4,5,6,7]
>>> result=zip(z1,z3)
>>> result
[(1, 4), (2, 5), (3, 6)]
```  
zip()配合*號操作符，可以將已經zip過的列表對象解壓  
``` python
>>> zip(*result)
[(1, 2, 3), (4, 5, 6)]
```  
更近一層了解  
``` python
* 二維矩陣變換（矩陣的行列互換）
比如我们有一個由列表描述的二維矩陣
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
通过python列表推导的方法，我们也能轻易完成这个任务
print [ [row[col] for row in a] for col in range(len(a[0]))]
[[1, 4, 7], [2, 5, 8], [3, 6, 9]]
另外一種讓人困惑的方法就是利用zip函數：
>>> a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> zip(*a)
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
>>> map(list,zip(*a))
[[1, 4, 7], [2, 5, 8], [3, 6, 9]]
zip函數接受任意多個序列作為參數，將所有序列按相同的索引組合成一個元素是各個序列合併成的tuple的新序列，新的序列的長度以參數中最短的序列為準。另外(*)操作符与zip函數配合可以實現與zip相反的功能，即將合併的序列拆成多個tuple。
①tuple的新序列
>>>>x=[1,2,3],y=['a','b','c']
>>>zip(x,y)
[(1,'a'),(2,'b'),(3,'c')]
②新的序列的長度以參數中最短的序列為準.
>>>>x=[1,2],y=['a','b','c']
>>>zip(x,y)
[(1,'a'),(2,'b')]
③(*)操作符与zip函數配合可以實現與zip相反的功能,即將合併的序列拆成多個tuple。
>>>>x=[1,2,3],y=['a','b','c']
>>>>zip(*zip(x,y))
[(1,2,3),('a','b','c')]
```  
[Origin](http://www.cnblogs.com/BeginMan/archive/2013/03/14/2959447.html)  
  
***3. Map***  
對sequence中的item依次執行function(item)，執行結果輸出為list。  
``` python
>>> map(str, range(5))           #對range(5)各項進行str操作
['0', '1', '2', '3', '4']        #返回列表
>>> def add(n):return n+n
... 
>>> map(add, range(5))           #對range(5)各項進行add操作
[0, 2, 4, 6, 8]
>>> map(lambda x:x+x,range(5))   #lambda 函數，各項+本身
[0, 2, 4, 6, 8]
>>> map(lambda x:x+1,range(10))  #lambda 函數，各項+1
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> map(add,'zhoujy')            
['zz', 'hh', 'oo', 'uu', 'jj', 'yy']
```  
想要輸入多個序列，需要支持多個參數的函數，注意的是各序列的長度必須一樣，否則報錯：  
``` python
>>> def add(x,y):return x+y
... 
>>> map(add,'zhoujy','Python')
['zP', 'hy', 'ot', 'uh', 'jo', 'yn']
>>> def add(x,y,z):return x+y+z
... 
>>> map(add,'zhoujy','Python','test')     #'test'的長度比其他2個小
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: add() takes exactly 2 arguments (3 given)
>>> map(add,'zhoujy','Python','testop')
['zPt', 'hye', 'ots', 'uht', 'joo', 'ynp']
```  
  
***4. Reduce***  
對sequence中的item顺序迭代調用function，函數必須要有2個參數。要是有第3個參數，則表示初始值，可以繼續调用初始值，返回一個值。  
``` python
>>> def add(x,y):return x+y
... 
>>> reduce(add,range(10))        #1+2+3+...+9
45
>>> reduce(add,range(11))        #1+2+3+...+10
55
>>> reduce(lambda x,y:x*y,range(1,3),5)           #lambda 函數，5是初始值， 1*2*5
10
>>> reduce(lambda x,y:x*y,range(1,6))             #阶乘，1*2*3*4*5
120
>>> reduce(lambda x,y:x*y,range(1,6),3)           #初始值3，结果再*3
360
>>> reduce(lambda x,y:x+y,[1,2,3,4,5,6])          #1+2+3+4+5+6
21
```  
  
***5. Filter***  
對sequence中的item依次執行function(item)，將執行結果為True（！=0）的item組成一個List/String/Tuple（取決于sequence的類型）返回，False則退出（0），進行過濾。  
``` python
>>> def div(n):return n%2
... 
>>> filter(div,range(5))                    #返回div輸出的不等於0的真值
[1, 3]
>>> filter(div,range(10))
[1, 3, 5, 7, 9]
>>> filter(lambda x : x%2,range(10))        #lambda 函數返回奇數，返回列表
[1, 3, 5, 7, 9]
>>> filter(lambda x : not x%2,range(10))
[0, 2, 4, 6, 8]
>>> def fin(n):return n!='z'                #過濾'z' 函數，出现z则返回False
... 
>>> filter(fin,'zhoujy')                    #'z'被過濾
'houjy'
>>> filter(lambda x : x !='z','zhoujy')     #labmda返回True值
'houjy'
>>> filter(lambda x : not x=='z','zhoujy')  #返回：字符串
'houjy'
```  
  
***6. Map, Reduce, Filter應用***  
*A 實現5!+4!+3!+2!+1*
``` python
#!/usr/bin/env python
#-*- coding:utf-8 -*-
def add_factorial(n):
    empty_list=[]           #聲明一個空列表，存各個階乘的結果，方便這些結果相加
    for i in map(lambda x:x+1,range(n)):    #用傳進來的變量(n)來生成一个列表，用map讓列表都+1，eg：range(5) => [1,2,3,4,5]
        a=reduce(lambda x,y:x*y,map(lambda x:x+1,range(i)))   #生成階乘，用map去掉列表中的0
        empty_list.append(a)            #把階乘結果append到空的列表中
    return empty_list
if __name__ == '__main__':
    import sys
#2選1
#(一)
    try:
        n = input("Enter a Number(int) : ")
        result=add_factorial(n)   #傳入變量
        print reduce(lambda x,y:x+y,result)      #階乘結果相加
    except (NameError,TypeError):
        print "That's not a Number!"
#(二)
#    result = add_factorial(int(sys.argv[1]))   #傳入變量
#    print reduce(lambda x,y:x+y,result)      #階乘結果相加
```  
  
*B 將100-200以內的質數挑選出來*  
1)  
``` python
#!/usr/bin/env python
#-*- coding:utf-8 -*-
def is_prime(start,stop):
    stop  = stop+1     #包含列表右边的值
    prime = filter(lambda x : not [x%i for i in range(2,x) if x%i == 0],range(start,stop))   #取出質數,x从range(start,stop) 取的數
    print prime
if __name__ == '__main__':
    try :
        start = input("Enter a start Number :")
    except :
        start = 2   #开始值默认2
    try :
        stop  = input("Enter a stop  Number :")
    except :
        stop  = 0   #停止數，默认0，即不返回任何值
    is_prime(start,stop)
```</br>
2)  
``` python
def prime(n):
    for s in range(2,n):
        if n % s == 0:
            return False
    return True

print filter(prime, range(100,201))
```

[Origin](http://www.cnblogs.com/zhoujinyi/archive/2013/06/07/3121976.html)