---
layout: post
title: "網頁重定向的方法(轉)"
date: 2014-04-02 10:58:43 +0800
comments: true
categories: web
keywords: webpage, 網頁，重定向，seo，方法
description: 網頁重定向的方法
---
####1. 使用HTTP通訊協定301 Moved Permanently來完成轉導網址(永久轉址)  
*建議使用，不會對SEO有不良影響*  
  
#####PHP程式範例:
``` php
<?php
    header(“HTTP/1.1 301 Moved Permanently");
    header(“Location: http://www.new-url.com/");
?>
<p>The document has moved <a href="http://www.new-url.com/">here</a>.</p>
```  <!--more-->
註  
1) 使用者的瀏覽器必須根據HTTP header的Location欄位值(稱做URI)來轉導網址。  
2) 除非Request Method是HEAD，不然伺服器端回覆的訊息內必須包含一短的新網址的連結(hyperlink)資訊。  
  
##### ASP程式範例:  
``` 
<%@ Language=VBScript %>
<%
    Response.Status="301 Moved Permanently"
    Response.AddHeader “Location", " http://www.new-url.com/"

    Response.End
%>
<p>The document has moved <a href="http://www.new-url.com/">here</a>.</p>
```  
  
##### ASP.NET程式範例:  
``` 
<script runat="server">

private void Page_Load(object sender, System.EventArgs e)
{
    Response.Status = “301 Moved Permanently";
    Response.AddHeader(“Location","http://www.new-url.com/");
}
</script>
<p>The document has moved <a href="http://www.new-url.com/">here</a>.</p>
```  
  
##### 在 .htaccess/httpd.conf檔案中設定—轉整domain  
```
Options +FollowSymLinks
RewriteEngine on
RewriteRule ^(.*) http://www.new-domain.com/$1 [R=301,L]
```  

##### 在 .htaccess/httpd.conf檔案中設定—轉到新的www.  
```
Options +FollowSymlinks
RewriteEngine on
RewriteCond %{http_host} ^old-domain.com [NC]

RewriteRule ^(.*)$ http://www.new-domain.com/$1 [R=301,NC]
```  
  
####2. 使用HTTP/1.1通訊協定302 Found來完成轉導網址  
*不建議使用，會對新網站SEO有不良影響*  
  
##### PHP程式範例:  
``` php
<?php
    header(“HTTP/1.1 302 Found");

    header(“Location: http://www.new-url.com/");
?>
<p>The document has moved <a href="http://www.new-url.com/">here</a>.</p>
```  

(…其他ASP, ASP.NET程式及設定.htaccess/httpd.conf方法，此處略…)  
  
註  
1) 302，在HTTP/1.0是『Moved Temporarily』；HTTP/1.1是『Found』，會根據HTTP header的Location欄位值(稱做URI)來轉導網址。但是很多網路上的文章會直接稱302是Moved Temporatily。  
2) 除非Request Method是HEAD，不然伺服器端回覆的訊息內必須包含一短的新網址的連結(hyperlink)資訊。  
3) HTTP 1.1中增訂了『307 Temporary Redirect』，307碼時只會根據GET Request轉導網址。  
4) 更多的HTTP 302細節和307會被再增訂出來的原因[請參考](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。
  
####3. HTML的refresh meta tag來轉導網址  
*非常不建議使用，會對新網站SEO有不良影響。有些文章寫說要用時最好秒數設定大於10秒以避免對頁面的SEO不利。*  
  
#####在HTML檔案的HEAD中，範例:  
``` html
<html>
<head>
    <meta http-equiv="refresh" content="0;url=http://www.new-url.com/" />
</head>
```  
  
####4. 用JavaScript來達到轉導網址(放在 HTML的<head>…</head>或<body>…<body>中  
*因為搜尋引擎的bot一般都不理會JavaScript，所以做什麼動作不會被檢查。這意味著要實做『點擊計算(click counting)後再轉導到目的網址的話，用這個方法比較好(302或refresh都是不好的方法)』。如果使用者按瀏覽器的『上一頁』按鈕，不會跳回轉導頁面。*  
  
#####直接在HTML的HEAD中用轉導網址JavaScript範例:
``` html
<html>
<head>
<script language="JavaScript">
<!–
    window.location.replace(“http://www.new-url.com");

//–>
</script>
</head>
</html>
```  
  
#####JavaScript內容同上例，但是把它放到外部的一個 .js 檔案，然後<head>…</head>中只要寫:  
``` html
<html>

<head>
    <script language="JavaScript" src="redirect.js"></script>
</head>
```  
  
#####也是使用JavaScript，但是額外透過『表單』來完成:
*因為搜尋引擎的bot一般都不理會『表單』，所以做什麼動作不會被檢查。*  
```
<script language="JavaScript">
<!–

    document.myform.submit();
//–>
</script>
<form name="myform" action="http://www.new-url.com/" method="get"></form>
```  
  
####額外討論:  
1. 301/302有時會被一些人用作旁門走道方法，在玩『PR劫持』(如[這篇文章](http://www.seozac.com/google/pr-hijack/)所述)，更多的一些手法討論請看[這篇文章](http://www.loriswebs.com/hijacking_web_pages.html)或用 hijack 當 KeyWord 去查查。  
2. 302在之前會造成bot誤以為是轉導到的網站在惡搞，而將轉導到的網站從索引中除名。所以會變得無法防止別人以此方法攻擊自己的 URL。現或許已更正。(詳情請看[這裡](http://www.tonyspencer.com/2004/12/10/tracker2php-pagejacking-via-http-302-redirect-google-bug/))  
3. 當然，refresh也能如上述302一樣去惡搞別人的網站。  
  
[Origin](http://rental.zhupiter.com/postshow_184_1_1.html)