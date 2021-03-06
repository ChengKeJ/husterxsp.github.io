---
layout: post
title: "学习react"
date: 2016-03-14 12:41:22
author: "Xsp"
header-img: "img/post-bg-default.jpg"
catalog: true
tags:
    - React
    - 前端
---


### 学习资料

+ 《react引领未来的用户界面开发框架》
+ [http://yiminghe.me/learning-react/tutorial/zh-cn/intro.html#/](http://yiminghe.me/learning-react/tutorial/zh-cn/intro.html#/ "http://yiminghe.me/learning-react/tutorial/zh-cn/intro.html#/")
+ [https://github.com/camsong/redux-in-chinese](https://github.com/camsong/redux-in-chinese "https://github.com/camsong/redux-in-chinese")


### JSX

+ javascript XML，在react内部构建标签的类XML的语法。优点：更加语义化和直观。
+ 动态值: 将两个花括号之间的内容渲染为动态值。
+ 子节点: react 将开始标签和结束标签之间的所有子节点都保存在this.props.children;
+ 非DOM属性
	+ key:为组件设置一个唯一的标识符，便于重用，提高渲染性能，
	+ ref:允许父组件在render方法之外保持对子组件的一个引用，通过this.ref.xxx获得引用，this.ref.xxx.getDOMNode获取真实的DOM节点
	+ dangerouslySetInnerHTML:将HTML内容设为字符串
+ 事件：react自动绑定了组件所有方法的作用域，不需要手动绑定
+ 注释：
	+ 作为子节点：包裹在花括号以内
	+ 作为内联属性：单行/多行
+ 特殊属性：由于JSX会转换为原生的javascript函数，因此一些关键词是不能用的，如for/class
+ 样式：驼峰形式
+ JSX对react不是必须，但会减少复杂性

### 组件的生命周期

实例化：

+ getDefaultProps: 对于组件类来说，仅调用一次
+ getInitialState： 对于组件的每个实例来说只会调用一次
+ componentWillMount: 可以修改state
+ render: 创建并返回一个虚拟的DOM, 表示组件的输出
+ componentDidMount: 真实DOM已经渲染，可以用this.getDOMNode()来调用（当react运行于服务端时，这个方法不会被调用？？？）

存在期：

+ componentWillReceiveProps： 组件的props通过父组件更改后，这个方法可以获得更改props对象以及更新state的机会
+ shouldComponentUpdate： 如果确定某个子组件或者它的任何父组件不需要渲染新的props或者state,则该方法返回false
+ componentWillUpdate: 
+ render
+ componentDidUpdate

销毁

+ componentWillUnmount: 在组件移除前调用，做一些清理工作，比如移除事件监听器

反模式
+ 把计算后的值传给state

### 数据流

react的数据流是单向的，从父节点传到子节点

Props
properties的缩写

+ 可以在组件或组件树外调用setProps, 但不能用this.setProps直接修改this.props, 可以通过this.props访问props，但不能用这种方式修改。一个组件不可以修改自己的props。
+ props可以用来添加事件处理器
+ PropTypes，验证props
+ getDefaultProps: 设置属性的默认值， 在createClass时调用，而不是在组件实例化时调用

State和props的区别是，props只存在于组件内部

+ state可以用来确定一个元素的视图状态
+ 可以通过setState赖修改，但不能直修改this.state
+ setState调用->render-> 虚拟DOM更新->真实DOM更新
+ 不要再state里保存计算出的值，而应该只保存最简单的数据，比如单选框是否勾选
+ 不要尝试把props复制到state, 要尽可能把props当作数据源

事件处理

+ 绑定： 虽然写法上类似普遍不推荐的HTML内联事件处理器属性，但底层实现并没有使用HTML的onClick
+ setState和replaceState, replaceState对象是会完全替换掉原来的state

mixin
定义可以在多个组件中共用的方法

待续。。