---
layout: post
title: "Notes of Python"
date: 2014-02-18 14:09:05 +0800
comments: true
categories: Python
---
1. Python 是怎樣進行類型轉換的？  
1) Python是動態類型，是强類型的編程語言。  
2) Python內建函數的實現類型轉換：  
		<table>
<tbody>
<tr><td><em> 函數 </em></td><td><em> 描述 </em></td></tr>  
<tr><td>int(x [,base ]) </td><td>將x轉換為一個整數</td></tr>
<tr><td>long(x [,base ]) </td><td>將x轉換為一個長整數</td></tr>
<tr><td>float(x) </td><td>將x轉換為一個浮點數</td></tr>
<tr><td>complax(real [, img ]) </td><td>創建一個複數</td></tr>
<tr><td>str(x) </td><td>將對象x轉換為字符串</td></tr>
<tr><td>repr(x) </td><td>將對象x轉換為表達式字符串</td></tr>
<tr><td>eval(str) </td><td>計算在字符串中的有效python表達式，並返回一個對象</td></tr>
<tr><td>tuple(s) </td><td>將序列s轉換為一個元組</td></tr>
<tr><td>list(s) </td><td>將序列s轉換為一個列表</td></tr>
<tr><td>chr(x) </td><td>將一個整數轉換為一個字符</td></tr>
<tr><td>unichr(x) </td><td>將一個整數轉換為一個Unicode字符</td></tr>
<tr><td>ord(x) </td><td>將一個字符轉換為它的整數值</td></tr>
<tr><td>hex(x) </td><td>將一個整數轉換為一個十六進制字符串</td></tr>
<tr><td>oct(x) </td><td>將一個整數轉換為一個八進制字符串</td></tr>
<tbody>
</table>  
  
2. range()函數的用法  
**range(start, stop[, step])**  
Example:  
``` python  
>>> range(10)  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]  
>>> range(1, 11)  
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  
>>> range(0, 30, 5)  
[0, 5, 10, 15, 20, 25]  
>>> range(0, 10, 3)  
[0, 3, 6, 9]  
>>> range(0, -10, -1)  
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]  
>>> range(0)  
[]  
>>> range(1, 0)  
[]  
```  
  
3. 生成隨機數  
1) **random.random**  
用於生成一個0到1的隨機浮點數：0<=n<1.0。  
2) **random.uniform**  
原型爲randon.uniform(a,b)，用於生成一個制定範圍內的隨機浮點數，兩個參數分別爲上下限：如果a>b，生成的浮點數：b<=n<a；如果a<b，生成的浮點數：a<=n<b。  
3) **random.randint**  
原型爲random.randint(a,b)，用於生成一個制定範圍內的整數。參數a爲下限，b爲上限，生成的隨機數：a<=n<=b。  
4) **random.randrange**  
原型爲random.randrange([start], stop[, step])，從指定範圍內，按指定基數递增的集合中獲取一個隨機數。如：random.randrange(10, 100, 2)，结果相當於從[10, 12, 14, 16, ... 96, 98]序列中獲取一個隨機數。random.randrange(10, 100, 2)在結果上於 random.choice(range(10, 100, 2)等效。  
5) **random.choice**  
原型爲random.choice(sequence)。參數sequence表示一個有序類型，其不是一種特定的類型，而是泛指一系列的類型，如list，tuple，string等。  
6) **random.shuffle**  
原型爲random.shufle(x[, random])，用於將一個列表中的元素打亂後輸出。  
7) **random.sample**  
原型爲random.sample(sequence, k)，從指定序列中隨機獲取制定長度的片段，sample函數不會修改原有序列。  
  
4. 如何查詢和替換一個文本字符串。  
1) sub()  
格式為sub(replacement, string[,count=0])  
replacement是被替換成的文本；  
string是需要被替換的文本；  
count是一個可選參數，指最大被替換的數量。  
2) subn()
執行的效果跟sub()一样，不過它會返回一個二維數組，包括替換後的新的字符串和總共替換的數量。