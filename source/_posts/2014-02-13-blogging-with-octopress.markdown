---
layout: post
title: "Blogging with Octopress"
date: 2014-02-13 21:32:44 +0800
comments: true
categories: 
---
  
1. 安裝Ruby  
2. 安裝Octopress  
1) 確保安裝了git，在終端輸入git --version可以看到計算機中的git版本。  
2) 利用命令將octopress從github上clone到本機：  
$ git clone git://github.com/imathis/octopress.git octopress  
$ cd octopress  
$ ruby --version  
安裝相關依賴項：  
$ sudo gem install bundler #需要輸入root密碼  
$ bundle install  
安裝默認的Octopress主題：  
$ rake install  <!--more-->
3. 配置Octopress  
$ vim ./_config.yml  
4. 將Blog部署到GitHub上  
在GitHub上創建一個倉庫，命名為username.github.com。等全部配置完後，可以通過在瀏覽器中輸入\http://username.github.com\來訪問。一般來說，將Blog的源碼放在source分支，把生成的內容提交到master分支。  
創建好倉庫之後，需要利用octopress的一個配置rake任務來自動配置上面創建的倉庫：  
$ rake setup_github_pages  
上面的命令最主要的是創建一個\_deploy目錄，用來存放部署到master分支的內容。期間會要求你輸入倉庫的url，根據提示，去GitHub.com上複製粘貼即可。  
完成上面的命令之後，我們就可以生成Blog並真正部署到倉庫中了。執行如下命令：  
$ rake generate  
$ rake deploy  
上面的命令首先生成Blog文件，並將生成的Blog文件拷貝到\_deploy 目錄下，然後將這些內容添加到git中，並commit和push到倉庫的master分支。  
現在可以訪問\http://username.github.com\了。注意，由於會發生延時，要等大約10分鐘才能打開。至此，我們已經基本完成對Blog的部署，不過Blog的source要單獨提交。  
5. 開始寫Blog  
Octopress為我們提供了一些task來創建Blog和頁面。博文必須存儲在source/\_posts 目錄下，並且需要按照Jekyll的命名規範對文章進行命名：YYYY-MM-DD-post-title.markdown。文章的名字會被當作url的一部分，而其中的日期用於對博文的區分和排序。創建博文命令爲：  
$ rake new_post["title"]  
然後在source/\_posts/YY-MM-DD-post-title.markdown中寫博文。  
之後，按照如下命令部署：  
$ rake generate  
$ git add .  
$ git commit -am "Some comments here."  
$ git push origin source  
$ rake deploy  
6. 添加頁面(pages)  
在Octopress中，有兩個默認的頁面，即blog/archives，可以參考它來完成自己的頁面。首先在source中創建一個目錄，例如author，然後在這個目錄中新建一個名為index.html的文件，根據需要進行編輯這個文件。  
重要的是，我們需要在首頁將這個頁面的鏈接顯示出來，此時需要編輯source/_includes/custom/navigation.html，仿照已有的內容添加一個新行，指向新創建的目錄即可。  
當rake generate正常之後，就可以rake deploy到GitHub上了。
7. 域名  
1) 在域名管理中，建立一個CNAME指向，將你的域名指向username.github.com  
2) 見一個名為CNAME的文件在source目錄下，然後將自己的域名輸入進去。  
3) 將內容push到GitHub後，第一次生效大概需要一個小時，之後就可以用自己的域名進行訪問了。  
8. 安裝模板  
常用第三方模板有：  
[3rd Party Octopress Themes](http://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)  
進入選擇好的模板鏈接，根據說明進行安裝。(本Blog使用的模板爲：[CleanPress](https://github.com/macjasp/cleanpress))   
9. 參考  
[Octopress Help](http://octopress.org/help/)  
[利用Octopress搭建一個GitHub博客](http://justcoding.iteye.com/blog/1954645)  
[象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)  
[在Mac上从零开始搭建基于Github的Octopress博客](http://blog.segmentfault.com/yaashion_xiang/1190000000364677)  
[为octopress添加新的页面(page)](http://icodeit.org/2013/01/add-new-page-to-octopress/)  
[我的Octopress配置](http://www.yanjiuyanjiu.com/blog/20130402/)  
[Octopress侧边栏及评论系统定制](http://blog.csdn.net/lcliliil/article/details/13725895)  
[N-blog](https://github.com/nswbmw/N-blog/wiki/_pages)  
