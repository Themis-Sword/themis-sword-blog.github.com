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