---
layout:     post
title: "跨域问题解决"
date: 2015-08-07 12:32:47
author:   "Xsp"
header-img: "img/post-bg-default.jpg"
tags:
    - 前端
    - JavaScript
---


跨域问题原因：浏览器的同源策略，即：同domain（或ip）, 同端口, 同协议视为同一个域, 一个域内的脚本仅仅具有本域内的权限，可以理解为本域脚本只能读写本域内的资源，而无法访问其它域的资源。这种安全限制称为同源策略。 
协议，子域名，端口号等不同，均会导致跨域问题 `No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.`
，有几种解决办法如下：


+ 使用代理：如果想访问不同域名下的api接口文件，可以访问同域名下的php文件，然后，该同域名下的php文件再来访问那个不同域名下的文件，即通过后端代理。


+ 使用jsonp，但jsonp仅支持GET请求，不支持POST请求，如下代码中即使写的是POST，但是通过查看network发送的请求可以发现，实际发送的是GET请求。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
<script type="text/javascript">
    $.ajax({ 
        type: "POST",   
        url: "http:husterxsp.sinaapp.com/jsonp_jquery.php",
        dataType: "jsonp",
        jsonp: "callback",
        success: function(data){
            console.log(data);
        }  
    });
</script>
</body>
</html>
```
```php
<?php
$data = '{"a":1,"b":3}';
$jsonp = $_GET['callback'];
echo $jsonp.'('.$data.')';
?>
```

原生js实现如下:
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>XHR</title>
</head>
<body>
<script type="text/javascript">
    function callfunc(data){
        console.log(data);
    }
    var url = "http:husterxsp.sinaapp.com/jsonp_js.php?&jsonp=callfunc";
    var script = document.createElement('script');
    script.setAttribute('src', url);
    document.getElementsByTagName('head')[0].appendChild(script); 
</script>
</body>
</html>
```
```php
<?php
$jsonp = $_GET['jsonp'];
$data = '{"a":1,"b":3}';
$result = $jsonp.'('.$data.')';
echo $result;

```
jsonp实现方式简单来说，就是通过javascript加载php，php返回一个带有所需数据的函数。
jsonp实现跨域的原理：script标签可以跨域(大概浏览器认为这个是安全的是因为直接引入一个文件，不会对服务器的文件的安全性有影响，但ajax可能会发送不利于服务器安全性的数据所以不能跨域？)，类似的可以跨域的还有link, img, iframe标签等等(有src及href属性)

+ 跨域资源共享CORS(Cross Origin Resource Sharing)：在后端php代码中添加：`header('Access-Control-Allow-Origin:*');`或者`header('Access-Control-Allow-Methods:POST,GET');`等等。浏览器会发送一个跨域的HTTP请求，请求的响应必须包含一个Access- Control-Allow-Origin的HTTP响应头，该响应头声明了请求域的可访问权限。

+ 另外，在调试的时候，可以在chrome属性，目标 ，添加 --disable-web-securit， 即可调试。
+ 跨域发送cookie, 需要设置withcredentials: true

---

以上是前端页面请求服务器获取数据的解决办法，另外一种就是页面与页面之间的跨域。解决办法：

+ 通过document.domain来跨子域
+ 通过window.name来进行跨域
+ 通过window.postMessage
+ window.navigator
+ location.hash

参考
+ [https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy "https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy")
+ [http://blog.csdn.net/kongjiea/article/details/38867531](http://blog.csdn.net/kongjiea/article/details/38867531 "http://blog.csdn.net/kongjiea/article/details/38867531")
+ [http://blog.csdn.net/kongjiea/article/details/44201021](http://blog.csdn.net/kongjiea/article/details/44201021 "http://blog.csdn.net/kongjiea/article/details/44201021")
+ [http://harttle.com/2015/10/10/cross-origin.html?utm_source=tuicool&utm_medium=referral](http://harttle.com/2015/10/10/cross-origin.html?utm_source=tuicool&utm_medium=referral "http://harttle.com/2015/10/10/cross-origin.html?utm_source=tuicool&utm_medium=referral")
