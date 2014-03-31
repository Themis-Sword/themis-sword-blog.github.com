---
layout: post
title: "Python os模塊 &amp; sys模塊"
date: 2014-03-31 16:06:01 +0800
comments: true
categories: python
---
**1. os模塊**  
Python os模塊包含普遍的操作系統功能。如果你希望你的程序能夠與平臺無關的話，這個模塊是尤為重要的。  
  
1) os.name  
輸出字符串指示正在使用的平臺。如果是window 則用'nt'表示，對於Linux/Unix用戶，它是'posix'。  
2) os.getcwd()  
函數得到當前工作目錄，即當前Python腳本工作的目錄路徑。  
3) os.listdir()  
返回指定目錄下的所有文件和目錄名。
``` python
>>> import os
>>> os.listdir(os.getcwd())
['.bash_history', '.bundler', '.CFUserTextEncoding', '.config', '.DS_Store', '.gem', '.gitconfig', '.matplotlib', '.ssh', '.Trash', '.Trash-500', '.vim', '.viminfo', 'Applications', 'Applications (Parallels)', 'Desktop', 'Documents', 'Downloads', 'Library', 'Movies', 'Music', 'octopress', 'Pictures', 'Public', 'PycharmProjects', '\xe7\x99\xbe\xe5\xba\xa6\xe4\xba\x91\xe5\x90\x8c\xe6\xad\xa5\xe7\x9b\x98']
>>> 
```  <!--more-->
4) os.remove()  
刪除一個文件。  
5) os.system()  
運行shell命令。  
6) os.sep  
可以取代操作系統特定的路徑分隔符。  
7) os.linesep  
給出當前平台使用的行終止符。  
8) os.path.split()  
函數返回一個路徑的目錄名和文件名  
9) os.path.isfile()和os.path.isdir()函數  
分別檢驗給出的路徑是一個文件還是目錄，給出True或者False。  
10) os.path.exists()  
檢驗給出的路徑是否真實的存在，給出True或者False。  
11) os.path.abspath(name)  
獲得絕對路徑。  
12) os.path.normpath(path)  
規範path的字符串形式。  
13) os.path.getsize(name)  
獲得文件大小，如果name是目錄返回0L。  
14) os.path.splitext()  
分離文件名與擴展名。  
15) os.path.join(path,name)  
連接目錄與文件名或目錄。  
16) os.path.basename(path)  
返回文件名。  
17) os.path.dirname(path)  
返回文件路徑。  
  
**2. sys模塊**  
1) sys.argv  
命令行參數List，第一個元素是程序本身路徑。  
2) sys.modules.keys()  
返回所有已經導入的模塊列表。  
3) sys.exc_info()  
獲取當前正在處理的異常類,exc_type、4) exc_value、exc_traceback當前處理的異常詳細信息。  
5) sys.exit(n)  
退出程序，正常退出時exit(0)。  
6) sys.hexversion  
獲取Python解釋程序的版本值，16進制格式如：0x020403F0。  
7) sys.version  
獲取Python解釋程序的版本信息。  
8) sys.maxint  
最大的Int值。  
9) sys.maxunicode  
最大的Unicode值。  
10) sys.modules  
返回系統導入的模塊字段，key是模塊名，value是模塊。  
11) sys.path  
返回模塊的搜索路徑，初始化時使用PYTHONPATH環境變量的值。  
12) sys.platform  
返回操作系統平臺名稱。  
13) sys.stdout  
標準輸出。  
14) sys.stdin  
標準輸入。  
15) sys.stderr  
錯誤輸出。  
16) sys.exc_clear()  
用來清除當前線程所出現的當前的或最近的錯誤信息。  
17) sys.exec_prefix  
返回平臺獨立的python文件安裝的位置。  
18) sys.byteorder  
本地字節規則的指示器，big-endian平臺的值是'big',little-endian平臺的值是'little'。  
19) sys.copyright  
記錄python版權相關的東西。  
20) sys.api_version  
解釋器的C的API版本。  
21) sys.version_info  
``` python
>>> sys.version_info
(2, 4, 3, 'final', 0) 'final'表示最終,也有'candidate'表示候選，表示版本級別，是否有後繼的發行
```  
22) sys.displayhook(value)  
如果value非空，這個函數會把他輸出到sys.stdout，並且將他保存進\_\_builtin\_\_.\_.指在python的交互式解釋器裏，'\_'代表上次你輸入得到的結果，hook是鉤子的意思，將上次的結果鉤過來。  
23) sys.getdefaultencoding()  
返回當前你所用的默認的字符編碼格式。  
24) sys.getfilesystemencoding()  
返回將Unicode文件名轉換成系統文件名的編碼的名字。  
25) sys.setdefaultencoding(name)  
用來設置當前默認的字符編碼，如果name和任何一個可用的編碼都不匹配，拋出LookupError，這個函數只會被site模塊的sitecustomize使用，一旦別site模塊使用了，他會從sys模塊移除。  
26) sys.builtin_module_names  
Python解釋器導入的模塊列表。  
27) sys.executable  
Python解釋程序路徑。  
28) sys.getwindowsversion()  
獲取Windows的版本。  
29) sys.stdin.readline()  
從標準輸入讀一行。  
30) sys.stdout.write("a")  
屏幕輸出a。  