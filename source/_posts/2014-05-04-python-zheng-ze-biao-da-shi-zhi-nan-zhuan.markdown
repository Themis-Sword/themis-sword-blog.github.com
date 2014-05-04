---
layout: post
title: "Python 正則表達式指南(轉)"
date: 2014-05-04 14:49:04 +0800
comments: true
categories: python
keywords: python, 正則表達式, 字符, 語法, 匹配, re, 模塊
description: Python正則表達式
---
####1. 正則表達式基礎  
#####1.1 簡單介紹  
正則表達式並不是Python的一部分。正則表達式是用於處理字符串的強大工具，擁有自己獨特的語法以及一個獨立的處理引擎，效率上可能不如str自帶的方法，但功能十分強大。得益於這一點，在提供了正則表達式的語言裏，正則表達式的語法都是一樣的，區別只在於不同的編程語言實現支持的語法數量不同；但不用擔心，不被支持的語法通常是不常用的部分。如果已經在其他語言裏使用過正則表達式，只需要簡單看一看就可以上手了。  
  
下圖展示了使用正則表達式進行匹配的流程：<!--more-->  
{% img /images/zhengzbds.png %}  
  
正則表達式的大致匹配過程是：依次拿出表達式和文本中的字符比較，如果每一個字符都能匹配，則匹配成功；一旦有匹配不成功的字符則匹配失敗。如果表達式中有量詞或邊界，這個過程會稍微有一些不同，但也是很好理解的，看下圖中的示例以及自己多使用幾次就能明白。  
  
下錶列出了Python支持的正則表達式元字符和語法：  
<table><tbody>
<tr><td><em> 語法 </em></td><td><em> 說明 </em></td><td><em> 表達式實例 </em></td><td><em> 完整匹配的字符串 </em></td></tr>
<tr><td></td><td><td><em> 字符 </em></td></td><td></td></tr>
<tr><td> 一般字符 </td><td> 匹配自身 </td><td> abc </td><td> abc </td></tr>
<tr><td> . </td><td> 匹配任意除換行符"\n"外的字符。在DOTALL模式中也能匹配換行符 </td><td> a.c </td><td> abc </td></tr>
<tr><td> \ </td><td> 轉義字符，使後一個字符改變原來的意思。如果字符串中有字符*需要匹配，可以使用\*或者字符集[*] </td><td> a\.c a\\c </td><td> a.c a\c </td></tr>
<tr><td> [...] </td><td> 字符集(字符類)。對應的位置可以是字符集中任意字符。字符集中的字符可以逐個列出，也可以給出範圍，如[abc]或[a-c]。第一個字符如果是^則表示取反，如[^abc]表示不是abc的其他字符。 所有的特殊字符在字符集中都失去原油的特殊含義。在字符集中如果要使用]、-或^，可以在前面加上反斜槓，或把]、-放在第一個字符，把^放在非第一個字符 </td><td> a[bdc]e </td><td> abe ace ade </td></tr>
<tr><td></td><td><td><em> 預定義字符集(可以寫在字符集[...]中) </em></td></td><td></td></tr>
<tr><td> \d </td><td> 數字:[0-9] </td><td> a\dc </td><td> a1c </td></tr>
<tr><td> \D </td><td> 非數字:[^\d] </td><td> a\Dc </td><td> abc </td></tr>
<tr><td> \s </td><td> 非白字符:[<空格>\t\r\n\f\v] </td><td> a\sc </td><td> a c </td></tr>
<tr><td> \S </td><td> 非空白字符:[^\s] </td><td> a\Sc </td><td> abc </td></tr>
<tr><td> \w </td><td> 單詞字符:[A-Z a-z 0-9] </td><td> a\wc </td><td> abc </td></tr>
<tr><td> \W </td><td> 非單詞字符:[^\W] </td><td> a\Wc </td><td> a c </td></tr>
<tr><td></td><td><td><em> 數量詞(用在字符或(...)之後) </em></td></td><td></td></tr>
<tr><td> * </td><td> 匹配前一個字符0或無限次 </td><td> abc* </td><td> ab abccc </td></tr>
<tr><td> + </td><td> 匹配前一個字符1或無限次 </td><td> abc+ </td><td> abc abccc </td></tr>
<tr><td> ? </td><td> 匹配前一個字符0或1次 </td><td> abc? </td><td> ab abc </td></tr>
<tr><td> {m} </td><td> 匹配前一個字符m次 </td><td> ab{2}c </td><td> abbc </td></tr>
<tr><td> {m,n} </td><td> 匹配前一個字符m至n次。m和n可以省略：若省略m，則匹配0至n次；若省略n，則匹配m至無限次 </td><td> ab{1,2}c </td><td> abc abbc </td></tr>
<tr><td> \*?+? ?? {m,n}? </td><td> 使*+?{m,n}變成非貪婪模式 </td><td> 示例在下文中介紹 </td><td> </td></tr>
<tr><td></td><td><td><em> 邊界匹配(不消耗待匹配字符串中的字符) </em></td></td><td></td></tr>
<tr><td> ^ </td><td> 匹配字符串開頭。在多行模式中匹配每一行的開頭。 </td><td> ^abc </td><td> abc </td></tr>
<tr><td> $ </td><td> 匹配字符串末尾。在多行模式中匹配每一行的末尾。 </td><td> abc$ </td><td> abc </td></tr>
<tr><td> \A </td><td> 儘匹配字符串開頭。 </td><td> \Aabc </td><td> abc </td></tr>
<tr><td> \Z </td><td> 儘匹配字符串末尾。 </td><td> \Zabc </td><td> abc </td></tr>
<tr><td> \b </td><td> 匹配\w和\W之間。 </td><td> a\b!bc </td><td> a!bc </td></tr>
<tr><td> \B </td><td> [^\b] </td><td> a\Bbc </td><td> abc </td></tr>
<tr><td></td><td><td><em> 邏輯、分組 </em></td></td><td></td></tr>
<tr><td> | </td><td> |代表左右表達式任意匹配一個。總是先嘗試匹配左邊的表達式，一旦成功匹配則跳過匹配右邊的表達式。如果|沒有被包括在()中，則它的範圍是整個正則表達式。 </td><td> abc|def </td><td> abc def </td></tr>
<tr><td> (...) </td><td> 被擴起來的表達式將作為分組，從表達式左邊開始每遇到一個分組的左括號'('，編號+1。另外，分組表達式作為一個整體，可以後接數量詞。表達式中的|儘在該組中有效。 </td><td> (abc){2} a(123|456)c </td><td> abcabc a456c </td></tr>
<tr><td> (?P<name>...) </td><td> 分組，除了原有的編號外再指定一個額外的別名。 </td><td> (?P<id>abc){2} </td><td> abcabc </td></tr>
<tr><td> \<number> </td><td> 引用編號為<number>的分組匹配到的字符串 </td><td> (\d)abc\1 </td><td> 1abc1 5abc5 </td></tr>
<tr><td> (?P=name) </td><td> 引用別名為<name>的分組匹配到的字符串。 </td><td> (?P<id>\d)abc(?P=id) </td><td> 1abc 5abc5 </td></tr>
<tr><td></td><td><td><em> 特殊構造(不作為分組) </em></td></td><td></td></tr>
<tr><td> (?:...) </td><td> (...)的不分組版本，用於使用'|'或後接數量詞 </td><td> (?:abc){2} </td><td> abc abc </td></tr>
<tr><td> (?iLmsux) </td><td> iLmsux的每個字符串代表一個匹配模式，只能用在正則表達式的開頭，可選多個。匹配模式將在下文中介紹。 </td><td> (?i)(abc) </td><td> AbC </td></tr>
<tr><td> (?#...) </td><td> #後的內容將作為註釋被忽略 </td><td> abc(?#comment)123 </td><td> abc123 </td></tr>
<tr><td> (?=...) </td><td> 之後的字符串內容需要匹配表達式才能成功匹配。不消耗字符串內容。 </td><td> abc(?=\d) </td><td> 後面是數字的a </td></tr>
<tr><td> (?!...) </td><td> 之後的字符串內容需要不匹配表達式才能成功匹配。不消耗字符串內容。 </td><td> abc(?!\d) </td><td> 後面不是數字的a </td></tr>
<tr><td> (?<=...) </td><td> 之前的字符串內容需要匹配表達式才能成功匹配。不消耗字符串內容。 </td><td> (?<=\d)a </td><td> 前面是數字的a </td></tr>
<tr><td> (?< !...) </td><td> 之前的字符串內容需要不匹配表達式才能成功匹配。不消耗字符串內容。 </td><td> (?<!\d)a </td><td> 前面不是數字的a </td></tr>
<tr><td> (?(id/name)yes-pattern|no-pattern) </td><td> 如果編號為id/別名為name的組匹配到字符，則需要皮皮yes-pattern，否則需要匹配no-pattern。 |np-pattern可以省略。 </td><td> (\d)abc(?(1)\d|abc) </td><td> 1abc2 abcabc </td></tr>
</tbody></table>  
  
#####1.2 數量詞的貪婪模式與非貪婪模式  
正則表達式通常用於在文本中查找匹配的字符串。Python裏數量詞默認是貪婪的（在少數語言裏也可能是默認非貪婪），總是嘗試匹配盡可能多的字符；非貪婪的則相反，總是嘗試匹配盡可能少的字符。例如：正則表達式"a\*"如果用於查找"abbbc"，將找到"abbb"。而如果使用非貪婪的數量詞"ab*?"，將找到"a"。  
  
#####1.3. 反斜杠的困擾  
與大多數編程語言相同，正則表達式裏使用"\"作為轉義字符，這就可能造成反斜杠困擾。假如你需要匹配文本中的字符"\"，那麽使用編程語言表示的正則表達式裏將需要4個反斜杠"\\\\\\\"：前兩個和後兩個分別用於在編程語言裏轉義成反斜杠，轉換成兩個反斜杠後再在正則表達式裏轉義成一個反斜杠。Python裏的原生字符串很好地解決了這個問題，這個例子中的正則表達式可以使用r"\\\"表示。同樣，匹配一個數字的"\\\d"可以寫成r"\d"。有了原生字符串，你再也不用擔心是不是漏寫了反斜杠，寫出來的表達式也更直觀。  
  
#####1.4. 匹配模式  
正則表達式提供了一些可用的匹配模式，比如忽略大小寫、多行匹配等，這部分內容將在Pattern類的工廠方法re.compile(pattern[, flags])中一起介紹。  
  
####2. re模塊  
#####2.1 開始使用re  
Python通過re模塊提供對正則表達式的支持。使用re的一般步驟是先將正則表達式的字符串形式編譯為Pattern實例，然後使用Pattern實例處理文本並獲得匹配結果（一個Match實例），最後使用Match實例獲得信息，進行其他的操作。  
``` python
# encoding: UTF-8
import re
 
# 将正则表达式编译成Pattern对象
pattern = re.compile(r'hello')
 
# 使用Pattern匹配文本，获得匹配结果，无法匹配时将返回None
match = pattern.match('hello world!')
 
if match:
    # 使用Match获得分组信息
    print match.group()
 
### 输出 ###
# hello
```  
  
**re.compile(strPattern[, flag]):**  
這個方法是Pattern類的工廠方法，用於將字符串形式的正則表達式編譯為Pattern對象。 第二個參數flag是匹配模式，取值可以使用按位或運算符'|'表示同時生效，比如re.I | re.M。另外，你也可以在regex字符串中指定模式，比如re.compile('pattern', re.I | re.M)與re.compile('(?im)pattern')是等價的。 
可選值有：  
* re.I(re.IGNORECASE): 忽略大小寫（括號內是完整寫法，下同）  
* M(MULTILINE): 多行模式，改變'^'和'$'的行為（參見上圖）  
* S(DOTALL): 點任意匹配模式，改變'.'的行為  
* L(LOCALE): 使預定字符類 \w \W \b \B \s \S 取決於當前區域設定  
* U(UNICODE): 使預定字符類 \w \W \b \B \s \S \d \D 取決於unicode定義的字符屬性  
* X(VERBOSE): 詳細模式。這個模式下正則表達式可以是多行，忽略空白字符，並可以加入註釋。以下兩個正則表達式是等價的：  
``` python
a = re.compile(r"""\d +  # the integral part
                   \.    # the decimal point
                   \d *  # some fractional digits""", re.X)
b = re.compile(r"\d+\.\d*")
```  
re提供了眾多模塊方法用於完成正則表達式的功能。這些方法可以使用Pattern實例的相應方法替代，唯一的好處是少寫一行re.compile()代碼，但同時也無法復用編譯後的Pattern對象。這些方法將在Pattern類的實例方法部分一起介紹。如上面這個例子可以簡寫為：  
``` python
m = re.match(r'hello', 'hello world!')
print m.group()
```  
re模塊還提供了一個方法escape(string)，用於將string中的正則表達式元字符如*/+/?等之前加上轉義符再返回，在需要大量匹配元字符時有那麽一點用。  
  
#####2.2 Match  
Match对象是一次匹配的结果，包含了很多关于此次匹配的信息，可以使用Match提供的可读属性或方法来获取这些信息。  
  
属性：  
1) string: 匹配时使用的文本。  
2) re: 匹配时使用的Pattern对象。  
3) pos: 文本中正则表达式开始搜索的索引。值与Pattern.match()和Pattern.seach()方法的同名参数相同。  
4) endpos: 文本中正则表达式结束搜索的索引。值与Pattern.match()和Pattern.seach()方法的同名参数相同。  
5) lastindex: 最后一个被捕获的分组在文本中的索引。如果没有被捕获的分组，将为None。  
6) lastgroup: 最后一个被捕获的分组的别名。如果这个分组没有别名或者没有被捕获的分组，将为None。  
  
方法：  
1) **group([group1, …]):**   
获得一个或多个分组截获的字符串；指定多个参数时将以元组形式返回。group1可以使用编号也可以使用别名；编号0代表整个匹配的子串；不填写参数时，返回group(0)；没有截获字符串的组返回None；截获了多次的组返回最后一次截获的子串。  
2) **groups([default]):**  
以元组形式返回全部分组截获的字符串。相当于调用group(1,2,…last)。default表示没有截获字符串的组以这个值替代，默认为None。  
3) **groupdict([default]):**  
返回以有别名的组的别名为键、以该组截获的子串为值的字典，没有别名的组不包含在内。default含义同上。  
4) **start([group]):**  
返回指定的组截获的子串在string中的起始索引（子串第一个字符的索引）。group默认值为0。  
5) **end([group]):**  
返回指定的组截获的子串在string中的结束索引（子串最后一个字符的索引+1）。group默认值为0。  
6) **span([group]):**  
返回(start(group), end(group))。  
7) **expand(template):**  
将匹配到的分组代入template中然后返回。template中可以使用\id或\g<id>、\g<name>引用分组，但不能使用编号0。\id与\g<id>是等价的；但\10将被认为是第10个分组，如果你想表达\1之后是字符'0'，只能使用\g<1>0。  
``` python
import re
m = re.match(r'(\w+) (\w+)(?P<sign>.*)', 'hello world!')
 
print "m.string:", m.string
print "m.re:", m.re
print "m.pos:", m.pos
print "m.endpos:", m.endpos
print "m.lastindex:", m.lastindex
print "m.lastgroup:", m.lastgroup
 
print "m.group(1,2):", m.group(1, 2)
print "m.groups():", m.groups()
print "m.groupdict():", m.groupdict()
print "m.start(2):", m.start(2)
print "m.end(2):", m.end(2)
print "m.span(2):", m.span(2)
print r"m.expand(r'\2 \1\3'):", m.expand(r'\2 \1\3')
 
### output ###
# m.string: hello world!
# m.re: <_sre.SRE_Pattern object at 0x016E1A38>
# m.pos: 0
# m.endpos: 12
# m.lastindex: 3
# m.lastgroup: sign
# m.group(1,2): ('hello', 'world')
# m.groups(): ('hello', 'world', '!')
# m.groupdict(): {'sign': '!'}
# m.start(2): 6
# m.end(2): 11
# m.span(2): (6, 11)
# m.expand(r'\2 \1\3'): world hello!
```  
  
#####2.3 Pattern  
Pattern對象是一個編譯好的正則表達式，通過Pattern提供的一系列方法可以對文本進行匹配查找。  
Pattern不能直接實例化，必須使用re.compile()進行構造。  
Pattern提供了幾個可讀屬性用於獲取表達式的相關信息：  
1) pattern: 編譯時用的表達式字符串。  
2) flags: 編譯時用的匹配模式。數字形式。  
3) groups: 表達式中分組的數量。  
4) groupindex: 以表達式中有別名的組的別名為鍵、以該組對應的編號為值的字典，沒有別名的組不包含在內。  
``` python
import re
p = re.compile(r'(\w+) (\w+)(?P<sign>.*)', re.DOTALL)
 
print "p.pattern:", p.pattern
print "p.flags:", p.flags
print "p.groups:", p.groups
print "p.groupindex:", p.groupindex
 
### output ###
# p.pattern: (\w+) (\w+)(?P<sign>.*)
# p.flags: 16
# p.groups: 3
# p.groupindex: {'sign': 3}
```  
实例方法[ | re模块方法]：  
1) **match(string[, pos[, endpos]]) | re.match(pattern, string[, flags]):**  
这个方法将从string的pos下标处起尝试匹配pattern；如果pattern结束时仍可匹配，则返回一个Match对象；如果匹配过程中pattern无法匹配，或者匹配未结束就已到达endpos，则返回None。  
pos和endpos的默认值分别为0和len(string)；re.match()无法指定这两个参数，参数flags用于编译pattern时指定匹配模式。  
注意：这个方法并不是完全匹配。当pattern结束时若string还有剩余字符，仍然视为成功。想要完全匹配，可以在表达式末尾加上边界匹配符'$'。  
示例参见2.1小节。 
2) **search(string[, pos[, endpos]]) | re.search(pattern, string[, flags]):**  
这个方法用于查找字符串中可以匹配成功的子串。从string的pos下标处起尝试匹配pattern，如果pattern结束时仍可匹配，则返回一个Match对象；若无法匹配，则将pos加1后重新尝试匹配；直到pos=endpos时仍无法匹配则返回None。  
pos和endpos的默认值分别为0和len(string))；re.search()无法指定这两个参数，参数flags用于编译pattern时指定匹配模式。  
``` python
# encoding: UTF-8 
import re 
 
# 将正则表达式编译成Pattern对象 
pattern = re.compile(r'world') 
 
# 使用search()查找匹配的子串，不存在能匹配的子串时将返回None 
# 这个例子中使用match()无法成功匹配 
match = pattern.search('hello world!') 
 
if match: 
    # 使用Match获得分组信息 
    print match.group() 
 
### 输出 ### 
# world
```  
3) **split(string[, maxsplit]) | re.split(pattern, string[, maxsplit]):**  
按照能夠匹配的子串將string分割後返回列表。maxsplit用於指定最大分割次數，不指定將全部分割。  
``` python
import re
 
p = re.compile(r'\d+')
print p.split('one1two2three3four4')
 
### output ###
# ['one', 'two', 'three', 'four', '']
```  
4) **findall(string[, pos[, endpos]]) | re.findall(pattern, string[, flags]):**  
搜索string，以列表形式返回全部能匹配的子串。  
``` python
import re
 
p = re.compile(r'\d+')
print p.findall('one1two2three3four4')
 
### output ###
# ['1', '2', '3', '4']
```  
5) **finditer(string[, pos[, endpos]]) | re.finditer(pattern, string[, flags]):**  
搜索string，返回一個順序訪問每一個匹配結果（Match對象）的叠代器。  
``` python
import re
 
p = re.compile(r'\d+')
for m in p.finditer('one1two2three3four4'):
    print m.group(),
 
### output ###
# 1 2 3 4
```  
6) **sub(repl, string[, count]) | re.sub(pattern, repl, string[, count]):**  
使用repl替換string中每一個匹配的子串後返回替換後的字符串。  
當repl是一個字符串時，可以使用\id或\g<id>、\g<name>引用分組，但不能使用編號0。  
當repl是一個方法時，這個方法應當只接受一個參數（Match對象），並返回一個字符串用於替換（返回的字符串中不能再引用分組）。  
count用於指定最多替換次數，不指定時全部替換。  
``` python
import re
 
p = re.compile(r'(\w+) (\w+)')
s = 'i say, hello world!'
 
print p.sub(r'\2 \1', s)
 
def func(m):
    return m.group(1).title() + ' ' + m.group(2).title()
 
print p.sub(func, s)
 
### output ###
# say i, world hello!
# I Say, Hello World!
```  
7) **subn(repl, string[, count]) |re.sub(pattern, repl, string[, count]):**  
返回 (sub(repl, string[, count]), 替換次數)。  
``` python
import re
 
p = re.compile(r'(\w+) (\w+)')
s = 'i say, hello world!'
 
print p.subn(r'\2 \1', s)
 
def func(m):
    return m.group(1).title() + ' ' + m.group(2).title()
 
print p.subn(func, s)
 
### output ###
# ('say i, world hello!', 2)
# ('I Say, Hello World!', 2)
```  
   
[Origin](http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html)
