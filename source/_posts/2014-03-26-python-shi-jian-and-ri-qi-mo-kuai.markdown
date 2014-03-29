---
layout: post
title: "Python 時間&amp;日期模塊"
date: 2014-03-26 16:53:19 +0800
comments: true
categories: python
---
Python提供了time/datetime/calendar等模塊來處理日期和時間。  
**1.time模塊常用的函數**  
*A. time.time()*  
1970年1月1日以來的秒數，是一個浮點數。  
*B. time.sleep()*  
可以通過調用time.sleep來挂起當前的進程。time.sleep接收一个浮點型參數，表示進程挂起的時間。  
*C. time.clock()*  
time.clock()返回第一次调用该方法到现在的秒數，其精確度高於1微秒。可以使用该函數來記錄程序執行的時間。 <!--more-->  
*D. time.gmtime()*  
該函數原型為time.gmtime([sec])，可選參數sec表示從1970-01-01以來的秒數，默認值為time.time()，函數返回time.struct_time類型的對象。(struct_time是在time模塊中定義的表示時間的對象)，下面是一個簡單的例子：  
``` python
>>> print time.gmtime()                        #獲取當前時間的struct_time對象   
(2013, 4, 8, 4, 28, 30, 0, 98, 0)
>>> print time.gmtime(time.time())
(2013, 4, 8, 4, 28, 50, 0, 98, 0)
>>> print time.gmtime(time.time()-24*60*60)    #獲取昨天這個時間的struct_time對象
(2013, 4, 7, 4, 29, 10, 6, 97, 0)
```  
*E. time.localtime()*  
與time.gmtime()非常類似，也返回一個struct_time對象，可以看作是gmtime()的本地化版本。  
*F. time.mktime()*  
time.mktime執行與gmtime(), localtime()相反的操作，它接收struct_time對象作為參數，返回用秒數來表示時間的浮點數。  
*G. time.strftime()*  
time.strftime將日期轉換為字符串表示，它的函數原型為：time.strftime(format[, t])。參數format是格式字符串（格式字符串的知識可以參考：[time.strftime](http://docs.python.org/2/library/time.html) ），可選的參數t是一个struct_time對象。  
``` python
time.strftime參數:
strftime(format[, tuple]) -> string
將指定的struct_time(默認為當前時間)，根據指定的格式化字符串輸出
python中時間日期格式化符號：
%y 两位數的年份表示（00-99）
%Y 四位數的年份表示（000-9999）
%m 月份（01-12）
%d 月内中的一天（0-31）
%H 24小時製小時數（0-23）
%I 12小時製小時數（01-12）
%M 分鐘數（00=59）
%S 秒（00-59）
%a 本地簡化星期名稱
%A 本地完整星期名稱
%b 本地簡化的月份名稱
%B 本地完整的月份名稱
%c 本地相應的日期表示和時間表示
%j 年内的一天（001-366）
%p 本地A.M.或P.M.的等價符
%U 一年中的星期數（00-53），星期天為星期的開始
%w 星期（0-6），星期天為星期的開始
%W 一年中的星期數（00-53）星期一為星期的開始
%x 本地相應的日期表示
%X 本地相應的時間表示
%Z 當前時區的名稱
%% %號本身 
```  
*H. time.strptime()*
按指定格式解析一個表示时间的字符串，返回struct_time對象。該函數原型為：time.strptime(string, format)，两個參數都是字符串。  
``` python
>>> print time.strptime('2013-04-09 12:30:25','%Y-%m-%d %H:%M:%S')
(2013, 4, 9, 12, 30, 25, 1, 99, -1)
```  
  
**2. datetime模塊**  
*A. 兩個常量*  
datetime.MINYEAR和datetime.MAXYEAR，分別表示datetime所能表示的最小、最大年份。其中，MINYEAR=1,MAXYEAR=0000。  
*B. 幾個重要的類*  
1) datetime.date:表示日期的類，常用的屬性有tear, month, day:  
year的返回在兩個常量之間；  
month的範圍是[1,12]，月份是從1開始；  
day依據month來決定。   
2) date類提供了常用的類方法和類屬性：  
date.max, date.min：date對象所能表示的最大、最小日期；
date.today()：返回一個表示當前本地日期的date對象。  
``` python
>>> from datetime import *
>>> import time
>>> print date.today()
2014-03-26
>>> print date.max
9999-12-31
>>> print date.min
0001-01-01
```  
3) date提供的實例方法和屬性  
date.year、date.month、date.day：年、月、日；  
date.replace(year, month, day)：生成一个新的日期對象，用參數指定的年，月，日代替原有對象中的屬性。（原有對象仍保持不變）；  
date.timetuple()：返回日期對應的time.struct_time對象；  
date.weekday()：返回weekday，如果是星期一，返回0；如果是星期2，返回1，以此類推；  
data.isoweekday()：返回weekday，如果是星期一，返回1；如果是星期2，返回2，以此類推；  
date.isocalendar()：返回格式如(year，month，day)的元組；  
date.isoformat()：返回格式如'YYYY-MM-DD’的字符串；  
date.strftime(fmt)：自定義格式化字符串。  
``` python
>>> now=date(2014,03,26)
>>> tomorrow=now.replace(day=27)
>>> print 'now:',now
now: 2014-03-26
>>> print 'tomorrow:',tomorrow
tomorrow: 2014-03-27
>>> print 'timetuple():',now.timetuple()
timetuple(): (2013, 4, 8, 0, 0, 0, 2, 85, -1)
>>> print 'weekday():',now.weekday()
weekday(): 2
>>> print 'isoweekday():',now.isoweekday()
isoweekday(): 3
>>> print 'isocalendar():',now.isocalendar()
isocalendar(): (2014, 13, 3)
>>> print 'isoformat():',now.isoformat()
isoformat(): 2014-03-26
```   
4) date還對某些操作進行了重載，它允許我們對日期進行如下一些操作：  
date2 = date1 + timedelta  # 日期加上一個間隔，返回一個新的日期對象（timedelta將在下面介紹，表示時間間隔）
date2 = date1 - timedelta   # 日期隔去間隔，返回一個新的日期對象
timedelta = date1 - date2   # 两個日期相減，返回一個時間間隔對象
date1 < date2  # 两個日期進行比較  
``` python
now = date.today()  
tomorrow = now.replace(day = 7 )  
delta = tomorrow - now  
print   'now:' , now,  ' tomorrow:' , tomorrow  
print   'timedelta:' , delta  
print  now + delta  
print  tomorrow > now  
# # ---- output ----   
# now: 2010-04-06  tomorrow: 2010-04-07   
# timedelta: 1 day, 0:00:00   
# 2010-04-07   
# True
```  
[Origin](http://www.cnblogs.com/BeginMan/archive/2013/04/08/3007403.html)