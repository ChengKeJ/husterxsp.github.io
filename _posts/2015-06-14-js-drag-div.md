---
layout: post
title: "js 实现拖拽效果"
date: 2015-06-14 13:19:53
author:  "Xsp"
header-img: "img/post-bg-default.jpg"
tags:
    - 前端
    - JavaScript
---

js onmousedown，onmousemove，onmouseup实现简单的拖拽：
```html
<!docType html>
<html>
<head>
<style type="text/css">
    #drag {
        position: relative; /*必须设置position*/
        width: 100px;
        height: 100px;
        background-color: #ABC;
    }
</style>
</head>
<body>
    <div id="drag"></div>
    <script>
        var drag = document.getElementById('drag');
        drag.onmousedown = dragHandle;
        function dragHandle(e) {
            var deltaX = e.clientX - drag.offsetLeft;
            var deltaY = e.clientY - drag.offsetTop;
            document.onmousemove = moveHandle;
            document.onmouseup = upHandle;
            function moveHandle(e) {
                drag.style.left = (e.clientX - deltaX) + "px";
                drag.style.top = (e.clientY - deltaY) + "px";
            }
            function upHandle() {
                document.onmousemove = null;
                document.onmouseup = null;
            }
        }
    </script>
</body>
</html>
```
demo: [http://codepen.io/husterxsp/pen/qdjzzp](http://codepen.io/husterxsp/pen/qdjzzp "http://codepen.io/husterxsp/pen/qdjzzp")
