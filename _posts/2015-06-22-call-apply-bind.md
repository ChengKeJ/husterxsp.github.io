---
layout: post
title: "call,apply,bind方法"
date: 2015-06-22 13:12:19
author:  "Xsp"
header-img: "img/post-bg-default.jpg"
tags:
    - 前端
    - JavaScript
---

apply，call的作用就是在特定的作用域中调用函数。
+ apply接收两个参数，一个是在其中运行函数的作用域，另一个是参数数组，其中第二个参数可以是Array的实例，也可以是arguments对象，例如：
```javascript
    function sum (num1,num2) {
        return num1+num2;
    }
    function callSum1(sum1,sum2) {
        return sum.apply(this,arguments);
        //this对象指定当前函数执行环境为callSum1,因此，此处将this换为callSum1也一样
    }
    function callSum2(sum1,sum2) {
        return sum.apply(this,[sum1,sum2]);
    }
    console.log(callSum1(10,10));   //20
    console.log(callSum2(10,10));   //20
```
+ call方法与apply的区别在于它们接收参数得方式不同，对于call方法，第一个参数是this，变化的是其余参数都直接传递给函数，也就是说传递给函数的参数必须逐个列举出，例如：
```javascript
    function sum (num1,num2) {
        return num1+num2;
    }
    function callSum1(sum1,sum2) {
        return sum.call(this,sum1,sum2);
    }

    console.log(callSum1(10,10));   //20
```
+ bind方法会创建一个函数的实例，其this值会指向传给bind方法的对象。
```javascript
    var color = "red";
    var o = {color:"blue"};

    function sayColor() {
        console.log(this.color);
    }
    var objectSayColor = sayColor.bind(o);
    objectSayColor();
```
以上内容均参考《javascript高级程序设计》Function类型章节。
