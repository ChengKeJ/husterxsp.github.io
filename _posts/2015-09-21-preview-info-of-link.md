---
layout:     post
title:     "meta标签自定义链接预览信息"
date:    2015-09-21 21:37:42
author:   "Xsp"
header-img: "img/post-bg-default.jpg"
tags:
    - 前端
    - HTML
---

在写好一个页面，用qq发送链接时，预览信息可能是这样的
![](http://i.imgur.com/ELnEf9P.png)

它的预览信息，并不是自己想要的效果。但是想到qq空间的分享，可以预览信息，看了一下qq空间的代码
![](http://i.imgur.com/W6WS03b.png)

发现它是itemprop属性来定义一些预览信息的。亲测有效。但是这样写，好像qq消息预览时只会显示第一次发出消息时的预览消息，也就是说，如果你已经qq发送了一个链接，接下来，你再修改meta里对应的信息，发送出去的链接信息预览仍然是第一次的时候的

另外还有一种办法是
```
<div style="display: none">
    <img src="img/logo.png" alt="">
</div>
```
将上面的这段代码放到紧随body之后，便可以在预览显示logo.png。此方法在微信分享链接时也是有效的，另外微信JSSDK可以自定义分享信息，但是好像需要微信认证，没有试过。有兴趣的话可以参看[怎么使用微信JSSDK的自定义分享功能](http://www.huceo.com/post/414.html)