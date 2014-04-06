---
layout: post
title: "Python with語句(轉)"
date: 2014-04-06 16:37:46 +0800
comments: true
categories: python
keywords: python, with, statement
description: understanding python's with statement
---

**1. With 語句是什麼**  
Python’s with statement provides a very convenient way of dealing with the situation where you have to do a setup and teardown to make something happen. A very good example for this is the situation where you want to gain a handler to a file, read data from the file and the close the file handler.  
有一些任務，可能事先需要設置，事後做清理工作。對於這種場景，Python的with語句提供了一種非常方便的處理方式。一個很好的例子是文件處理，你需要獲取一個文件句柄，從文件中讀取數據，然後關閉文件句柄。<!--more-->  
  
Without the with statement, one would write something along the lines of:  
如果不用with語句，代碼如下：   
``` python
file = open("/tmp/foo.txt")
data = file.read()
file.close()
```  
There are two annoying things here. First, you end up forgetting to close the file handler. The second is how to handle exceptions that may occur once the file handler has been obtained. One could write something like this to get around this:  
這裏有兩個問題。一是可能忘記關閉文件句柄；二是文件讀取數據發生異常，沒有進行任何處理。下面是處理異常的加強版本：  
``` python
file = open("/tmp/foo.txt")
try:
    data = file.read()
finally:
    file.close()
```  
While this works well, it is unnecessarily verbose. This is where with is useful. The good thing about with apart from the better syntax is that it is very good handling exceptions. The above code would look like this, when using with:  
雖然這段代碼運行良好，但是太冗長了。這時候就是with一展身手的時候了。除了有更優雅的語法，with還可以很好的處理上下文環境產生的異常。下面是with版本的代碼：  
``` python
with open("/tmp/foo.txt") as file:
    data = file.read()
```  
  
**2. with如何工作**  
while this might look like magic, the way Python handles with is more clever than magic. The basic idea is that the statement after with has to evaluate an object that responds to an \_\_enter\_\_() as well as an \_\_exit\_\_() function.  
這看起來充滿魔法，但不僅僅是魔法，Python對with的處理還很聰明。基本思想是with所求值的對象必須有一個\_\_enter\_\_()方法，一個\_\_exit\_\_()方法。  
  
After the statement that follows with is evaluated, the \_\_enter\_\_() function on the resulting object is called. The value returned by this function is assigned to the variable following as. After every statement in the block is evaluated, the \_\_exit\_\_() function is called.  
緊跟with後面的語句被求值後，返回對象的\_\_enter\_\_()方法被調用，這個方法的返回值將被賦值給as後面的變量。當with後面的代碼塊全部被執行完之後，將調用前面返回對象的\_\_exit\_\_()方法。  
  
This can be demonstrated with the following example:  
下面例子可以具體說明with如何工作：  
``` python
#!/usr/bin/env python
# with_example01.py
 
 
class Sample:
    def __enter__(self):
        print "In __enter__()"
        return "Foo"
 
    def __exit__(self, type, value, trace):
        print "In __exit__()"
 
 
def get_sample():
    return Sample()
 
 
with get_sample() as sample:
    print "sample:", sample
```  
When executed, this will result in:  
運行代碼，輸出如下  
``` python
bash-3.2$ ./with_example01.py
In __enter__()
sample: Foo
In __exit__()
```  
As you can see,  
The \_\_enter\_\_() function is executed  
The value returned by it - in this case "Foo" is assigned to sample  
The body of the block is executed, thereby printing the value of sample ie. "Foo"  
The \_\_exit\_\_() function is called.  
What makes with really powerful is the fact that it can handle exceptions. You would have noticed that the \_\_exit\_\_() function for Sample takes three arguments - val, type and trace. These are useful in exception handling. Let’s see how this works by modifying the above example.  
正如你看到的，  
1) \_\_enter\_\_()方法被執行  
2) \_\_enter\_\_()方法返回的值 - 這個例子中是"Foo"，賦值給變量'sample'  
3) 執行代碼塊，打印變量"sample"的值為 "Foo"  
4) \_\_exit\_\_()方法被調用  
with真正強大之處是它可以處理異常。可能你已經註意到Sample類的\_\_exit()\_\_方法有三個參數- val, type 和 trace。 這些參數在異常處理中相當有用。我們來改一下代碼，看看具體如何工作的。  
``` python
#!/usr/bin/env python
# with_example02.py
 
 
class Sample:
    def __enter__(self):
        return self
 
    def __exit__(self, type, value, trace):
        print "type:", type
        print "value:", value
        print "trace:", trace
 
    def do_something(self):
        bar = 1/0
        return bar + 10
 
with Sample() as sample:
    sample.do_something()
```  
Notice how in this example, instead of get_sample(), with takes Sample(). It does not matter, as long as the statement that follows with evaluates to an object that has an \_\_enter\_\_() and \_\_exit\_\_() functions. In this case, Sample()’s \_\_enter\_\_() returns the newly created instance of Sample and that is what gets passed to sample.  
這個例子中，with後面的get_sample()變成了Sample()。這沒有任何關系，只要緊跟with後面的語句所返回的對象有\_\_enter\_\_()和\_\_exit\_\_()方法即可。此例中，Sample()的\_\_enter\_\_()方法返回新創建的Sample對象，並賦值給變量sample。  
  
When executed:  
代碼執行後：  
``` python
bash-3.2$ ./with_example02.py
type: <type 'exceptions.ZeroDivisionError'>
value: integer division or modulo by zero
trace: <traceback object at 0x1004a8128>
Traceback (most recent call last):
  File "./with_example02.py", line 19, in <module>
    sample.do_something()
  File "./with_example02.py", line 15, in do_something
    bar = 1/0
ZeroDivisionError: integer division or modulo by zero
```  
Essentially, if there are exceptions being thrown from anywhere inside the block, the \_\_exit\_\_() function for the object is called. As you can see, the type, value and the stack trace associated with the exception thrown is passed to this function. In this case, you can see that there was a ZeroDivisionError exception being thrown. People implementing libraries can write code that clean up resources, close files etc. in their \_\_exit\_\_() functions.  
實際上，在with後面的代碼塊拋出任何異常時，\_\_exit\_\_()方法被執行。正如例子所示，異常拋出時，與之關聯的type，value和stack trace傳給\_\_exit\_\_()方法，因此拋出的ZeroDivisionError異常被打印出來了。開發庫時，清理資源，關閉文件等等操作，都可以放在\_\_exit\_\_方法當中。  
  
Thus, Python’s with is a nifty construct that makes code a little less verbose and makes cleaning up during exceptions a bit easier.  
因此，Python的with語句是提供一個有效的機制，讓代碼更簡練，同時在異常產生時，清理工作更簡單。  
  
I have put the code examples given here on [Github](https://github.com/sdqali/python_dojo/tree/master/with).  
示例代碼可以在[Github](https://github.com/sdqali/python_dojo/tree/master/with)上面找到。  
  
譯註：本文原文見[Understanding Python's "With" Statement](http://blog.sdqali.in/blog/2012/07/09/understanding-pythons-with/)  
  
[Origin](http://python.42qu.com/11155501)