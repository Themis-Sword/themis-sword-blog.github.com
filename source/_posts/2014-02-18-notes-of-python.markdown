---
layout: post
title: "Notes of Python 01"
date: 2014-02-18 14:09:05 +0800
comments: true
categories: python
keywords: python，類型，range()，序列，字符串
description: python類型轉換
---
**1. Python 是怎樣進行類型轉換的？**  
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
</tbody>
</table> <!--more-->  
  
**2. range()函數與xrange()函數的用法**  
01) **range()**  
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
  
02) **xrange()**  
用法與range()完全相同，不同的是生成的不是一個數組，而是一個生成器。  
Example:  
``` python
>>> xrange(5)
xrange(5)
>>> list(xrange(5))
[0, 1, 2, 3, 4]
>>> xrange(1,5)
xrange(1, 5)
>>> list(xrange(1,5))
[1, 2, 3, 4]
>>> xrange(0,6,2)
xrange(0, 6, 2)
>>> list(xrange(0,6,2))
[0, 2, 4]
```  
  
03) **區別**  
要生成很大的數字序列的時候，用xrange會比range性能優很多，因為不需要一上來就開闢一塊很大的內存空間，這兩個基本上都是在循環的時候用：  
``` python
for i in range(0, 100): 
print i 
for i in xrange(0, 100): 
print i 
```  
這兩個輸出的結果都是一樣的，實際上有很多不同，range()會直接生成一個list對象：  
``` python
>>> a = range(0,100) 
>>> print type(a) 
>>> print a 
>>> print a[0], a[1] 
```  
輸出結果：  
``` python
>>> <type 'list']] >
>>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
>>> 0 1
```  
而xrange()不會直接生成一个list，而是每次調用返回其中的一個值：  
``` python
>>> a = xrange(0,100) 
>>> print type(a) 
>>> print a 
>>> print a[0], a[1]
```   
輸出結果：
``` python
>>> <type 'xrange']] >
>>> xrange(100)
>>> 0 1
```  
  
因此可見，xrange()做循環的性能比range()好，尤其是返回很大的時候，儘量用xrange()，除非返回的是一個列表。  
  
**3. 生成隨機數**  
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
  
**4. 如何查詢和替換一個文本字符串。**  
1) sub()  
格式為sub(replacement, string[,count=0])  
replacement是被替換成的文本；  
string是需要被替換的文本；  
count是一個可選參數，指最大被替換的數量。  
2) subn()
執行的效果跟sub()一样，不過它會返回一個二維數組，包括替換後的新的字符串和總共替換的數量。  
  
5. 兩個序列的和的差最小  
有两個序列a,b，大小都爲n,序列元素的值任意整数，無序；  
要求：通過交換a,b 中的元素，使[序列a 元素的和]与[序列b 元素的和]之間的差最小。  
例如:  
var a=[100,99,98,1,2,3];  
var b=[1,2,3,4,5,40];  
**分析：**  
當數組a和b的和之差爲A = sum(a) - sum(b)，a的第i個元素和b的第j個元素交換後，a和b的和之差爲：  
A' = sum(a) - a[i] + b[j] - (sum(b) - b[j] + a[i])  
   = sum(a) - sum(b) - 2 (a[i] - b[j])  
   = A - 2 (a[i] - b[j])  
設 x = a[i] - b[j]  
|A| - |A'| = |A| - |A - 2x|  
假設A > 0，  
當x在(0, A)之間時，做這樣的交換才能使得交換後的a和b的和之差變小，x越接近A/2效果越好，如果找不到在(0, A)之间的x，則當前的a和b就是答案。  
所以大概算法如下：在a和b中尋找使得x在(0, A)之間，並且最接近A/2的i和j，交換相應的i和j元素，重新計算A後，重複前面的步骤直到找不到(0, A)之間的x為止。  
``` python
def mean(a, b):  
    if sum(a) < sum(b):  
        array = a  
        a = b  
        b = array  
    diff_sum = sum(a) - sum(b)  
    loop = True  
    while loop:  
        loop = False  
        md = diff_sum / 2  
        for i in range(0, len(a)):  
            for j in range(0, len(b)):  
                x = a[i] - b[j]  
                if x < diff_sum and x > 0:  
                    loop = True  
                    if abs(x - diff_sum / 2) < md:  
                        md = abs (x - diff_sum / 2)  
                        mi = i  
                        mj = j  
        if loop:  
            tmp = a[mi]  
            a[mi] = b[mj]  
            b[mj] = tmp  
            diff_sum = diff_sum - 2 * (b[mj] - a[mi])  
            if diff_sum < 0:  
                array = a  
                a = b  
                b = array  
                diff_sum = - diff_sum  
def main():  
    a = [7, 9, 10]  
    b = [6, 2, 8]  
    mean(a, b)  
    print (a)  
    print (b)  
if __name__ == '__main__':  
    main()  
```
[Origin](http://www.smallqiao.com/31.html)