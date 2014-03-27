---
layout: post
title: "Python import與from...import...(轉)"
date: 2014-03-27 14:23:52 +0800
comments: true
categories: Python
---
**1. 簡單說說python import與from...import....(python模塊)**
  
在python用import或者from...import來導入相應的模塊。模塊其實就一些函數和類的集合文件，它能實現一些相應的功能，當我們需要使用這些功能的時候，直接把相應的模塊導入到我們的程序中，我們就可以使用了。這類似於C語言中的include頭文件，Python中我們用import導入我們需要的模塊。  
``` python
import sys
print('================Python import mode==========================');
print ('The command line arguments are:')
for i in sys.argv:
    print (i)
print ('\n The python path',sys.path)

from sys import argv,path #導入特定的成員
print('================python from import===================================')
print('path:',path)

如果你要使用所有sys模塊使用的名字，你可以這樣：

from sys import *
print('path:',path)
```  
從以上我們可以簡單看出：

\==========  
導入mode，import與from...import的不同之處在於，簡單說：
如果你想要直接輸入argv變量到你的程序中而每次使用它時又不想打sys，則可使用：from sys import argv。  
一般說來，應該避免使用from..import而使用import語句，因為這樣可以使你的程序更加易讀，也可以避免名稱的沖突。  
\==========  
  
在使用 from xxx import * 時，如果想精準的控制模塊導入的內容，可以使用 __all__ = [xxx,xxx] 來實現，例如：  
``` python
__all__ = ['a','b'] #__為雙橫線
class two():
    def __init__(self):
        print('this is two')
a = 'this is two a'
b = 'this is two b'
if __name__=='__main__':
    t = two()

one.py

from two import *
print a
print b
t = two()

這時，類two()將不會被 import *導入進來
```  
  
**2. 關於import中的路徑搜索問題**  
類似於頭文件，模塊也是需要系統的搜索路徑的，下面的命令即是系統默認的搜索路徑，當你導入一個模塊時，系統就會在下面的路徑列表中搜索相應的文件。  
``` python
>>> print(sys.path)

['', '/usr/lib64/python26.zip', '/usr/lib64/python2.6', '/usr/lib64/python2.6/plat-linux2', '/usr/lib64/python2.6/lib-tk', '/usr/lib64/python2.6/lib-old', '/usr/lib64/python2.6/lib-dynload', '/usr/lib64/python2.6/site-packages', '/usr/lib64/python2.6/site-packages/gst-0.10', '/usr/lib64/python2.6/site-packages/gtk-2.0', '/usr/lib64/python2.6/site-packages/webkit-1.0', '/usr/lib/python2.6/site-packages']

(從例中，我們可以看到python會首先在當前工作目錄裏去找)
```  
  
如果沒有找導相應的內容，則報錯：  
``` python
>>> import syssTraceback (most recent call last):  File "<stdin>", line 1, in <module>ImportError: No module named syss

當然，我們也可以自行添加要搜索的路徑，調用列表的append方法即可：
import sys
sys.path.append('/usr/xx/python2.6')
```  
  
**3. 創建自己的模塊**  
在創建之前，有一點需要說明一下：每個Python模塊都有它的\_\_name\_\_（就每個對象都自己的\_\_doc\_\_一樣）。通過__name__我們可以找出每一個模塊的名稱，一般\_\_name\_\_的值有種：1 一是主模塊名稱為："\_\_main\_\_"(可以理解為直接運行的那個文件)，2 那些被主模塊導入的模塊名稱為：文件名字（不加後面的.py）。有\_\_name\_\_是很有用的，因為我們可以通過 if \_\_name\_\_  == 'xxx' 判斷來執行那些模塊，那些模塊不被執行。另外：每個Python程序也是一個模塊。它擴展名為：.py擴展名。  
  
下面，我們通過例子來說明：  
首先：我們創建模塊：mymodel.py  
``` python
#!/user/bin/python
#Filename:mymodel.py
version = '1.0'
def sayHello():
    print ('Hello world')

def modelName():
    return __name__#返回它自己的名稱
#end of model
```  
以上語句注意：  
1) 這個模塊應該被放置在我們輸入它的程序的同一個目錄中，或者在sys.path所列目錄之一。  
2) 你已經看到，它與我們普通的Python程序相比並沒有什麽特別之處。  
然後：我們在test.py中來調用此模塊：test.py  
``` python
import sys,mymodel
sys.path.append('D:/xx/PythonSERVER/python31/Code')#提供搜索路径
print(__name__) #此處打印主模塊的名稱：__main__
mymodel.sayHello()
print('Version',mymodel.version)
print('Model Name',mymodel.modelName())#打印被導入模塊的名稱: mymodel

我們使用from..import...

print('======================from.....import=====================================')
from mymodel import *
print(__name__)#此處打印主模塊的名稱：__main__
sayHello()
print('Version',version)
print('Model Name',modelName()) #打印被導入模塊的名稱: mymodel
```  
以上語句註意：
1) 我們可以通過import來導入多個模塊，用“,”（逗號）分隔。  
2) 註意import與from..import.....  
  
**4. 創建自己的包**  
*A 一個包的基本組織如下：*  
``` python
FC/
  __init__.py
  Libr/
    __init__.py
    one.py
    two.py
    ....
  Model/
    __init__.py
    one.py
    ....


在外部加載調用時，有以下方式：
#coding:utf-8
#加載方式一
import Fc.Libr.one
print Fc.Libr.one.a
#加載方式二
from Fc.Libr import one
print one.a
#加載方式三
from Fc.Libr.one import a
print a
#加載方式四
from Fc.Libr import *
print one.a
註意直接使用第四種方式是不能正確導入Libr下的one子模塊的，這就需要在Fc目錄下的__init__.py文件中定義好需要加載子模塊的名稱
Fc/Libr/__init__.py
__all__=['one','two'] #定義加載子模塊的名稱
```  
在加載包模塊時，在import語句執行期時，遇到的所有\_\_init\_\_.py文件都會被執行，在上面代碼中
首先會執行Fc目錄中的\_\_int\_\_.py，然後執行Libr目錄中的\_\_init\_\_.py  
  
*B 子模塊加子模塊問題*  
同一包的相同目錄中：  
``` python
#coding:utf-8
#加載方式一:使用完全限定名稱
from Fc.Libr import one
aa = 'libr two load one---'+one.a

#加載方式二:使用相對導入
from . import one
bb = 'libr two load one----'+one.b
方二中使用.來表示在同一級目錄中。


#加載方式三:(這種方式應當避免:最後找不到會轉移到標準庫)
import Fc.Libr.one
cc = 'libr two load one---'+Fc.Libr.one.a

在外部使用時：
#coding:utf-8
from Fc.Libr import *
print two.aa
print two.bb


同一包的不同目錄中：

#coding:utf-8
from ..Model import one
a = 'libr two load mode one---'+one.a

使用時：
#coding:utf-8
from Fc.Libr import *
print two.a
將輸出：libr two load mode one---fc model one
```  
  
另外在導入一個包時，會定義一個特殊的變量\_\_path\_\_，該變量包含一個目錄列表。  
\_\_path\_\_可通過\_\_init\_\_.py文件中包含的代碼訪問，最初包含的一項具有包的目錄名稱。我們可以向\_\_path\_\_列表提供更多的目錄，以更改查找子模塊時使用的搜索路徑，大型項目中這個很有用。  
  
**5. 特別說明**  
1) import執行加載源文件中所有語名（所以模塊是一個文件）。  
2) import語句可以出現在程序中的任何位置。但是有一點是：無論import語句被使用了多少次，每個模塊中的代碼僅加載和執行一次，後續的import語句僅將模塊名稱綁定到前一次導入所創建的模塊對象上。  
3) 使用sys.modules可查看當前加載的所有模塊。  

[Origin](http://www.cnblogs.com/ptfblog/archive/2012/07/15/2592122.html)