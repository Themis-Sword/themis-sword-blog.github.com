---
layout: post
title: "Python 字符串"
date: 2014-03-28 01:15:00 +0800
comments: true
categories: python
---
**1. 轉義字符**  
<table>
<tbody>
<tr><td><em>轉義字符</em></td><td><em>描述</em></td></tr>  
<tr><td>\\(在行尾)</td><td>續行符</td></tr>  
<tr><td>\\\ </td><td>反斜槓</td></tr>  
<tr><td>\'</td><td>單引號</td></tr>  
<tr><td>\"</td><td>雙引號</td></tr>  
<tr><td>\a</td><td>響鈴</td></tr>  
<tr><td>\b</td><td>退格(Backspace)</td></tr>  
<tr><td>\e</td><td>轉義</td></tr>  
<tr><td>\000</td><td>空</td></tr>  
<tr><td>\n</td><td>換行</td></tr>  
<tr><td>\v</td><td>縱向製表符</td></tr>  
<tr><td>\t</td><td>橫向製表符</td></tr>  
<tr><td>\r</td><td>回車</td></tr>  
<tr><td>\f</td><td>換頁</td></tr>  
<tr><td>\oyy</td><td>八進制數yy代表的字符，例如:\o12代表換行</td></tr>  
<tr><td>\xyy</td><td>十進制數yy代表的字符，例如:\x0a代表換行</td></tr>  
<tr><td>\uhhhh</td><td>Unicode 16位的十六進製值</td></tr>  
<tr><td>\uhhhhhhhh</td><td>Unicode 32位的十六進製值</td></tr>  
<tr><td>\other</td><td>其它的字符以普通格式輸出</td></tr>  
</tbody></table><!--more-->
  
**2. 索引和切片**  
Python中的字符串是有序的字符集合，所以可以通過位置（索引）獲取對應的元素。和c語言中一樣，python的索引也是從0開始的，而且支持使用負索引的方法來獲取元素，一個負的索引可以看做是從字符串結尾處反向計數，-1就表示字符串的最後一個字符。當然也可以理解為負索引與字符串長度相加得到的正索引，即s[-n]等於s[-n+len(s)]。  
``` python
>>> s = "hello"  
>>> s[0]  
'1'  
>>> s = "hello"  
>>> s[0]  
'h'  
>>> s[-1]  
'o'  
>>> s[0:3]  
'hel'  
>>> s[1:]  
'ello'  
>>> s[:-1]  
'hell'
```  
其實分片的完整格式為s[start:end:side]，意思就是從start開始到end-1，每隔side個元素取一個元素，返回值為所有取到的元素組成的字符串，side默認值為1。  
side可以取負值，例如s[::-1]會返回”olleh”，實際效果就是對字符串進行了反轉。這裏需要註意的是，如果side為負值，兩個邊界也要進行反轉，s[4:1:-1]就是從4開始反向取到2得到的字符串，如果在sride為負的情況下還是第一個邊界大於第二個邊界那樣的使用的話將返回一個空的字符串。  
``` python
>>> s[::2]  
'hlo'  
>>> s[::-1]  
'olleh'  
>>> s[4:1:-1]  
'oll'  
>>> s[0:4:-1]  
''  
```  
  
**3. 字符串方法**  
*A 大小寫*  
S.upper() 全部大寫  
S.lower() 全部小寫  
S.swapcase() 大小寫互換  
S.capitalize() 首字母大寫，其余都小寫  
S.title() 每個單詞的首字母大寫，其余不變  
  
*B 對齊*  
S.ljust(width[, fill]) 獲取固定長度width，左對齊，多余位用fill填充，默認空格  
S.rjust(width[, fill]) 右對齊  
S.center(width[, fill]) 居中  
S.zfill(width) 右對齊，左邊不足的位置用0補齊，相當於S.rjust(width,"0")  
如果width<=len(S)，返回於原字符串相同的字符串。  
  
*C 查找替換*  
S.find(sub[, start[, end]]) 返回在範圍start到end(不含end)中第一個sub的索引。  
S.rfind(sub[, start[, end]]) 從右邊開始查找，也就是返回在範圍start到end(不含end)中最後一個sub的索引。  
S.index(sub[, start[, end]])功能與find()相同，不同之處在於find()未找到的時候會返回-1，而index()會拋出異常。  
S.rindex(sub[, start[, end]])功能與rfind()相同，不同之處在於find()未找到的時候會返回-1，而index()會拋出異常。  
S.count(sub[, start[, end]]) 返回在範圍start到end(不含end)中sub的個數。  
start默認為0，end默認為len(S)。  
S.replace(old, new[, count])將字符串中的old替換為new，count為替換的次數，未指定的話就是替換所有。  
S.translate(table [,deletechars]) 刪除S中deletechars包含的字符，然後將剩下的字符用table定義的關系進行映射。table是string.maketrans()生成的。  
  
*D 去空白或指定字符*  
S.strip([chars]) 去除字符串S兩邊的chars,若chars未指定，則去除兩邊的空白，包括空格、\n、\f、\r、\t和\v。  
S.lstrip([chars]) 去除左邊的chars，未指定同上。  
S.rstrip([chars]) 去除右邊的chars，未指定同上。  
  
*E 分割和組合*  
S.split([sep [,maxsplit]]) 以sep為分隔符切割字符串S，不指定sep默認為默認為空白，maxsplit為最大分割次數，未指定則全部分割，返回列表。  
S.rsplit([sep [,maxsplit]]) 只是和split反向相反，split從頭到尾，rsplit從尾到頭。  
S.splitlines([keepends]) 按行分割字符串，若keepends指定並且為True，則保留換行符，反之不保留。  
S.partiton(sep)以sep作為分隔符將S分割為兩個start和end部分，並返回(start,sep, end)這樣格式的元組，若sep沒有找到，則返回S和兩個空串組成的元組(S, '', '')。  
S.rpartition(sep)從右邊開始，以sep作為分隔符將S分割為兩個start和end部分，並返回(start,sep, end)，若sep沒有找到，則返回兩個空串和S組成的元組('', '', S)。  
S.join(iterable) 將叠代器iterable的字符串連接在一起，並用分隔符S隔開，一般來說在連接列表的時候都使用空字符串或者空格作為分隔符，返回字符串。  
  
*F 判斷*  
S.startswith(prefix[, start[, end]])判斷字符串start到end(不含end)是否是以prefix開頭，默認start為0，end為len(S)，返回布爾值。  
S.endswith(suffix[, start[, end]]) 判斷字符串start到end(不含end)是否是以suffix結尾，默認start為0，end為len(S)，返回布爾值。  
S.isalnum() 是否全為字母或數字，返回布爾值。  
S.isalpha() 是否全為字母，返回布爾值。  
S.isdigit() 是否全為數字0-9，返回布爾值。  
S.islower() 是否全是小寫。  
S.isupper() 是否全是大寫。  
S.isspace() 是否全是空白。  
S.istitle() 是否符合title的格式——每個單詞的首字母大寫。  
上述這些函數中，若S為空串，則均返回False。  
  
*G 編碼*  
S.decode([encoding[,errors]]) 將字符串S解碼為unicode，encoding為S原來的編碼方式。  
S.encode([encoding[,errors]]) 將unicodeS編碼為python中的字符串，參數指定對應的編碼方式。  
errors指定出錯時對應的操作，默認的strict會在編碼/解碼失敗的時候拋出異常，ignore則忽略。  
關於編碼方式還是有很多的知識的，後續再做研究。  
  
*H 其他*  
S.format(*args, **kwargs)  
format也是用來格式化字符串的，argv指定的變量可以在S中用{index}來替換，而kwargs則是對應的變量。如果後面的數據為字典，可以使用{<index|var>[key]}來替換其對應的值。當然還可以指定對應的寬度、精度以及對其格式，{index:[fill][<|>|^][width][.precision][typecode]}。  
fill設置填充位，默認為空格；<左對齊，>右對齊，^居中；寬度和類型見後面格式化表達式部分。註意這裏的精度專指說浮點型數據，整型是不能使用精度的，字符創設置了也沒啥作用。  
``` python
>>> "{0},{2},{1},{s}".format(1,2,3,s="four")  
'1,3,2,four'  
>>> "{0:$<3},{2:^5.2f},{1},{s:>10.6s}".format(1,2,3,s="four")  
'1$$,3.00 ,2,      four'  
>>> "{0:<3},{2:^5d},{1},{s:>10s}".format(1,2,3,s="four")  
'1  ,  3  ,2,      four'  
>>> "{0:<03},{2:^05.2d},{1},{s:>10s}".format(1,2,3,s="four")  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
ValueError: Precision not allowed in integer format specifier  
>>> "{0:<03},{2:^5.2f},{1},{s:>10.6s}".format(1,2,3,s="four")  
'100,3.00 ,2,      four' 
```  
S.expandtabs([tabsize]) 將字符串中的制表符替換為tabsize個空格，默認為8。  
  
*I 3.0新增*  
S.isdecimal() 是否全是十進制數字(多語言數字)。  
S.isidentifier() 是否全是合法標識符。  
S.isnumeric() 是否只包含數字字符。  
S.isprintable() 是否全是可打印字符。  
  
**4. 格式化表達**  
<table><tbody>  
<tr><td><em>格式化表達</em></td><td><em>描述</em></td></tr>  
<tr><td>%s</td><td>字符串</td></tr>  
<tr><td>%r</td><td>repr輸出的字符串</td></tr>  
<tr><td>%d</td><td>十進制整數</td></tr>  
<tr><td>%i</td><td>整數</td></tr>  
<tr><td>%u</td><td>無符號整數</td></tr>  
<tr><td>%o</td><td>八進制</td></tr>  
<tr><td>%x</td><td>十六進制</td></tr>  
<tr><td>%X</td><td>十六進制(大寫)</td></tr>  
<tr><td>%e</td><td>指數</td></tr>  
<tr><td>%E</td><td>指數(大寫)</td></tr>  
<tr><td>%f</td><td>十進制浮點數</td></tr>  
<tr><td>%F</td><td>十進制浮點數(大寫)</td></tr>  
<tr><td>%g</td><td>浮點e或f</td></tr>  
<tr><td>%G</td><td>浮點E或F</td></tr>  
</tbody></table>  
