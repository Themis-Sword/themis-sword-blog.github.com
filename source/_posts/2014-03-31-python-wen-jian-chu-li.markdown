---
layout: post
title: "Python 文件處理"
date: 2014-03-31 17:48:17 +0800
comments: true
categories: python
---
python對文件的處理的兩個內建函數：  
open()、file()，這個兩函數提供了初始化輸入\輸出（I\O）操作的通用接口。兩函數的功能相同。  
基本用法：  
file_object=open(filename, access_mode='r', buffering=-1） 
file_object 是定義一個打開文件的對象  
access_mode 是打開文件的模式；通常，文件使用模式  'r','w','a' 來打開，分別代表，讀取，寫入，追加。  
'r' 模式打開已經存在的文件  
'w' 模式打開的文件若存在則首先清空，再加入內容。  
'a' 這個模式是追加內容到文件中<!--more-->  
註. 'b' 模式這個是打開二進制文件，對於unix-like/unix類型的系統'b'模式是可有可無的。  
buffering 訪問文件所采用的緩沖方式。其中0表示不緩沖，1表示只緩沖一行數據，任何其它大於1的值代表使用給定的值作為緩沖區大小。不給定此參數或者參數為負數都表示使用系統默認的緩沖機制。  
使用open打開文件之後一定記得調用close()關閉文件。  
  
常用的文件訪問方式如下：  
r        以讀方式打開  
rU或Ua   以讀方式打開同時提供通用換行符支持  
w        以寫方式打開  
a        以追加方式打開  
r+       以讀寫方式打開  
w+       以讀寫方式打開  
a+       以讀寫方式打開  
  
文件的輸入：  
python中有三個方法來處理文件內容的輸入：  
read() 一次讀取全部的文件內容。  
readline() 每次讀取文件的一行。  
readlines() 讀取文件的所有行，返回一個字符串列表。  
  
寫數據：  
``` python
file_object = open('thefile.txt', 'w')
file_object.write(all_the_text)
file_object.close( )
```  
  
寫入多行：  
`file_object.writelines(list_of_text_strings)`  
  
seek(offset,where):  默認值where=0表示從起始位置移動"offset"個字節，where=1表示從當前位置移動"offset"個字節，where=2表示從結束位置移動"offset"個字節。當有換行時，會被換行截斷。seek()無返回值，故值為None。  
  
tell():  文件的當前位置,即tell是獲得文件指針位置，受seek、readline、read、readlines影響，不受truncate影響。  
  
truncate(n):  從文件的首行首字符開始截斷，截斷文件為n個字符；無n表示從當前位置起截斷；截斷之後n後面的所有字符被刪除。其中win下的換行代表2個字符大小。  
``` python
      fso = open("f:\\a.txt",'w+')    #以w+方式，並非a方式打開文件，故文件原內容被清空
      print fso.tell()    #文件原內容被清空，故此時tell()=0

      fso.write("abcde\n")  #寫入文件abcde\n，因為換行\n占兩個字符，故共寫入7個字符
      print fso.tell()  #此時tell()=7

      fso.write("fghwm")  #又寫入文件fghwm，故此時文件共寫入7+5 =142個字符
      print fso.tell()  #此時tell()=12 

      fso.seek(1, 0)  #從起始位置即文件首行首字符開始移動1個字符
      print fso.tell()   #此時tell() =1

      print  fso.readline()  #讀取當前行，即文件的第1行，但是從第二個字符(tell()+1)開始讀，結果為:bcde。'若換成for讀取整個文件或read讀取整個文件則結果為bcdefghwm     
      
      print fso.tell()   #因為readline此時tell() =7,

      fso.truncate(8)  #從寫入後文件的首行首字符開始階段，截斷為8個字符，即abcde\nf，即文件的內容為：abcde\nf

      print fso.tell()   #tell() 依舊為7,並為受truncate(8)影響，但是此時文件內容為abcde\nf

      print  fso.readline()  #從tell()+1=8開始讀取，讀取當前行內容：f

      fso.close()
```
