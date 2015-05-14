---
layout: post
title: "Python數據分析入門(轉)"
date: 2015-05-14 10:46:30 +0800
comments: true
categories: python, artificial-intelligence
keywords: python, data analysis
discription: Python Getting started with data analysis
---
最近，[Analysis with Programming](http://alstatr.blogspot.com/)加入了[Planet Python](http://planetpython.org/)。作為該網站的首批特約博客，我這裏來分享一下如何通過Python來開始數據分析。具體內容如下：

1. 數據導入  
\** 導入本地的或者web端的CSV文件；
2. 數據變換；  
3. 數據統計描述；  
4. 假設檢驗  
\** 單樣本t檢驗；  
5. 可視化；  
6. 創建自定義函數。<!--more-->

###數據導入

這是很關鍵的一步，為了後續的分析我們首先需要導入數據。通常來說，數據是CSV格式，就算不是，至少也可以轉換成CSV格式。在Python中，我們的操作如下：

	import pandas as pd
	 
	# Reading data locally
	df = pd.read_csv('/Users/al-ahmadgaidasaad/Documents/d.csv')
	 
	# Reading data from web
	data_url = "https://raw.githubusercontent.com/alstat/Analysis-with-Programming/master/2014/Python/Numerical-Descriptions-of-the-Data/data.csv"
	df = pd.read_csv(data_url)

為了讀取本地CSV文件，我們需要pandas這個數據分析庫中的相應模塊。其中的read_csv函數能夠讀取本地和web數據。

###數據變換

既然在工作空間有了數據，接下來就是數據變換。統計學家和科學家們通常會在這一步移除分析中的非必要數據。我們先看看數據：

	# Head of the data
	print df.head()
	 
	# OUTPUT
	    Abra  Apayao  Benguet  Ifugao  Kalinga
	0   1243    2934      148    3300    10553
	1   4158    9235     4287    8063    35257
	2   1787    1922     1955    1074     4544
	3  17152   14501     3536   19607    31687
	4   1266    2385     2530    3315     8520
	 
	# Tail of the data
	print df.tail()
	 
	# OUTPUT
     Abra  Apayao  Benguet  Ifugao  Kalinga
	74   2505   20878     3519   19737    16513
	75  60303   40065     7062   19422    61808
	76   6311    6756     3561   15910    23349
	77  13345   38902     2583   11096    68663
	78   2623   18264     3745   16787    16900

對R語言程序員來說，上述操作等價於通過print(head(df))來打印數據的前6行，以及通過print(tail(df))來打印數據的後6行。當然Python中，默認打印是5行，而R則是6行。因此R的代碼head(df, n = 10)，在Python中就是df.head(n = 10)，打印數據尾部也是同樣道理。

在R語言中，數據列和行的名字通過colnames和rownames來分別進行提取。在Python中，我們則使用columns和index屬性來提取，如下：

	# Extracting column names
	print df.columns
	 
	# OUTPUT
	Index([u'Abra', u'Apayao', u'Benguet', u'Ifugao', u'Kalinga'], dtype='object')
	 
	# Extracting row names or the index
	print df.index
	 
	# OUTPUT
	Int64Index([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78], dtype='int64')

數據轉置使用T方法，

	# Transpose data
	print df.T
	 
	# OUTPUT
	            0      1     2      3     4      5     6      7     8      9  
	Abra      1243   4158  1787  17152  1266   5576   927  21540  1039   5424  
	Apayao    2934   9235  1922  14501  2385   7452  1099  17038  1382  10588  
	Benguet    148   4287  1955   3536  2530    771  2796   2463  2592   1064  
	Ifugao    3300   8063  1074  19607  3315  13134  5134  14226  6842  13828  
	Kalinga  10553  35257  4544  31687  8520  28252  3106  36238  4973  40140  
	 
	         ...       69     70     71     72     73     74     75     76     77 
	Abra     ...    12763   2470  59094   6209  13316   2505  60303   6311  13345  
	Apayao   ...    37625  19532  35126   6335  38613  20878  40065   6756  38902  
	Benguet  ...     2354   4045   5987   3530   2585   3519   7062   3561   2583  
	Ifugao   ...     9838  17125  18940  15560   7746  19737  19422  15910  11096  
	Kalinga  ...    65782  15279  52437  24385  66148  16513  61808  23349  68663  
	 
	            78 
	Abra      2623 
	Apayao   18264 
	Benguet   3745 
	Ifugao   16787 
	Kalinga  16900 
	 
	Other transformations such as sort can be done using <code>sort</code> attribute. Now let's extract a specific column. In Python, we do it using either <code>iloc</code> or <code>ix</code> attributes, but <code>ix</code> is more robust and thus I prefer it. Assuming we want the head of the first column of the data, we have

其他變換，例如排序就是用sort屬性。現在我們提取特定的某列數據。Python中，可以使用iloc或者ix屬性。但是我更喜歡用ix，因為它更穩定一些。假設我們需數據第一列的前5行，我們有：

	print df.ix[:, 0].head()
	 
	# OUTPUT
	0     1243
	1     4158
	2     1787
	3    17152
	4     1266
	Name: Abra, dtype: int64

順便提一下，Python的索引是從0開始而非1。為了取出從11到20行的前3列數據，我們有：

	print df.ix[10:20, 0:3]
	 
	# OUTPUT
	    Abra  Apayao  Benguet
	10    981    1311     2560
	11  27366   15093     3039
	12   1100    1701     2382
	13   7212   11001     1088
	14   1048    1427     2847
	15  25679   15661     2942
	16   1055    2191     2119
	17   5437    6461      734
	18   1029    1183     2302
	19  23710   12222     2598
	20   1091    2343     2654

上述命令相當於df.ix[10:20, ['Abra', 'Apayao', 'Benguet']]。

為了舍棄數據中的列，這裏是列1(Apayao)和列2(Benguet)，我們使用drop屬性，如下：

	print df.drop(df.columns[[1, 2]], axis = 1).head()
	 
	# OUTPUT
	    Abra  Ifugao  Kalinga
	0   1243    3300    10553
	1   4158    8063    35257
	2   1787    1074     4544
	3  17152   19607    31687
	4   1266    3315     8520

axis 參數告訴函數到底舍棄列還是行。如果axis等於0，那麽就舍棄行。

###統計描述
下一步就是通過describe屬性，對數據的統計特性進行描述：

	print df.describe()
	 
	# OUTPUT
	               Abra        Apayao      Benguet        Ifugao       Kalinga
	count     79.000000     79.000000    79.000000     79.000000     79.000000
	mean   12874.379747  16860.645570  3237.392405  12414.620253  30446.417722
	std    16746.466945  15448.153794  1588.536429   5034.282019  22245.707692
	min      927.000000    401.000000   148.000000   1074.000000   2346.000000
	25%     1524.000000   3435.500000  2328.000000   8205.000000   8601.500000
	50%     5790.000000  10588.000000  3202.000000  13044.000000  24494.000000
	75%    13330.500000  33289.000000  3918.500000  16099.500000  52510.500000
	max    60303.000000  54625.000000  8813.000000  21031.000000  68663.000000

###假設檢驗
Python有一個很好的統計推斷包。那就是scipy裏面的stats。ttest_1samp實現了單樣本t檢驗。因此，如果我們想檢驗數據Abra列的稻谷產量均值，通過零假設，這裏我們假定總體稻谷產量均值為15000，我們有：

	from scipy import stats as ss
	 
	# Perform one sample t-test using 1500 as the true mean
	print ss.ttest_1samp(a = df.ix[:, 'Abra'], popmean = 15000)
	 
	# OUTPUT
	(-1.1281738488299586, 0.26270472069109496)

返回下述值組成的元祖：  
* t : 浮點或數組類型  
t統計量  
* prob : 浮點或數組類型  
two-tailed p-value 雙側概率值

通過上面的輸出，看到p值是0.267遠大於α等於0.05，因此沒有充分的證據說平均稻谷產量不是150000。將這個檢驗應用到所有的變量，同樣假設均值為15000，我們有：

	print ss.ttest_1samp(a = df, popmean = 15000)
	 
	# OUTPUT
	(array([ -1.12817385,   1.07053437, -65.81425599,  -4.564575  ,   6.17156198]),
	 array([  2.62704721e-01,   2.87680340e-01,   4.15643528e-70,
	          1.83764399e-05,   2.82461897e-08]))

第一個數組是t統計量，第二個數組則是相應的p值。

###可視化
Python中有許多可視化模塊，最流行的當屬matpalotlib庫。稍加提及，我們也可選擇bokeh和seaborn模塊。之前的博文中，我已經說明了matplotlib庫中的盒須圖模塊功能。  
{% img /images/pda/1.jpg %}

	# Import the module for plotting
	import matplotlib.pyplot as plt
	 plt.show(df.plot(kind = 'box'))

現在，我們可以用pandas模塊中集成R的ggplot主題來美化圖表。要使用ggplot，我們只需要在上述代碼中多加一行，

	import matplotlib.pyplot as plt
	pd.options.display.mpl_style = 'default' # Sets the plotting display theme to ggplot2
	df.plot(kind = 'box')

這樣我們就得到如下圖表：
{% img /images/pda/2.jpg %}

比matplotlib.pyplot主題簡潔太多。但是在本博文中，我更願意引入seaborn模塊，該模塊是一個統計數據可視化庫。因此我們有：  
{% img /images/pda/3.jpg %}

	# Import the seaborn library
	import seaborn as sns
	 # Do the boxplot
	plt.show(sns.boxplot(df, widths = 0.5, color = "pastel"))

多性感的盒式圖，繼續往下看。  
{% img /images/pda/4.jpg %}

	plt.show(sns.violinplot(df, widths = 0.5, color = "pastel"))
{% img /images/pda/5.jpg %}

	plt.show(sns.distplot(df.ix[:,2], rug = True, bins = 15))
{% img /images/pda/6.jpg %}

	with sns.axes_style("white"):
	    plt.show(sns.jointplot(df.ix[:,1], df.ix[:,2], kind = "kde"))
{% img /images/pda/7.jpg %}

	plt.show(sns.lmplot("Benguet", "Ifugao", df))

###創建自定義函數
在Python中，我們使用def函數來實現一個自定義函數。例如，如果我們要定義一個兩數相加的函數，如下即可：

	def add_2int(x, y):
	    return x + y
	 
	print add_2int(2, 2)
	 
	# OUTPUT
	4

順便說一下，Python中的縮進是很重要的。通過縮進來定義函數作用域，就像在R語言中使用大括號{…}一樣。這有一個我們之前博文的例子：

1. 產生10個正態分布樣本，其中u=3和σ<sup>2</sup>=5
2. 基於95%的置信度，計算x̄和<img src="http://latex.codecogs.com/gif.latex?\bar{x}\mp&space;z_{a/}(2\frac{\sigma&space;}{\sqrt{n}})" title="\bar{x}\mp z_{a/}(2\frac{\sigma }{\sqrt{n}})" />;
3. 重復100次; 然後
3. 計算出置信區間包含真實均值的百分比

Python中，程序如下：

	import numpy as np
	import scipy.stats as ss
	 
	def case(n = 10, mu = 3, sigma = np.sqrt(5), p = 0.025, rep = 100):
	    m = np.zeros((rep, 4))
	 
	    for i in range(rep):
	        norm = np.random.normal(loc = mu, scale = sigma, size = n)
	        xbar = np.mean(norm)
	        low = xbar - ss.norm.ppf(q = 1 - p) * (sigma / np.sqrt(n))
	        up = xbar + ss.norm.ppf(q = 1 - p) * (sigma / np.sqrt(n))
 
	        if (mu > low) & (mu < up):
	            rem = 1
	        else:
	            rem = 0
 
	        m[i, :] = [xbar, low, up, rem]
	 
	    inside = np.sum(m[:, 3])
	    per = inside / rep
	    desc = "There are " + str(inside) + " confidence intervals that contain "
	           "the true mean (" + str(mu) + "), that is " + str(per) + " percent of the total CIs"
 
	    return {"Matrix": m, "Decision": desc}

上述代碼讀起來很簡單，但是循環的時候就很慢了。下面針對上述代碼進行了改進，這多虧了 Python專家，看我上篇博文的[15條意見](http://alstatr.blogspot.com/2014/01/python-and-r-is-python-really-faster.html#disqus_thread)吧。

	import numpy as np
	import scipy.stats as ss
	 
	def case2(n = 10, mu = 3, sigma = np.sqrt(5), p = 0.025, rep = 100):
	    scaled_crit = ss.norm.ppf(q = 1 - p) * (sigma / np.sqrt(n))
	    norm = np.random.normal(loc = mu, scale = sigma, size = (rep, n))
 
	    xbar = norm.mean(1)
	    low = xbar - scaled_crit
	    up = xbar + scaled_crit
 
	    rem = (mu > low) & (mu < up)
	    m = np.c_[xbar, low, up, rem]
 
	    inside = np.sum(m[:, 3])
	    per = inside / rep
	    desc = "There are " + str(inside) + " confidence intervals that contain "
	           "the true mean (" + str(mu) + "), that is " + str(per) + " percent of the total CIs"
	    return {"Matrix": m, "Decision": desc}

###更新

那些對於本文ipython notebook版本感興趣的，請點擊[這裏](http://nuttenscl.be/Python_Getting_Started_with_Data_Analysis.html)。這篇文章由[Nuttens Claude](https://twitter.com/NuttensC)負責轉換成 ipython notebook 。

關於作者: [Den](http://python.jobbole.com/author/dengcy/)

[Origin](http://alstatr.blogspot.ca/2015/02/python-getting-started-with-data.html)  
[中文譯文](http://python.jobbole.com/81133/)