---
layout: post
title: 远程调用-《分布式系统概念与设计》
date: 2018-02-20
author: "Xsp"
catalog: true
tags:
    - 分布式
---

分布式系统中两个主要的远成方法调用：
+ 远程过程调用（RPC）
+ 远程方法调用（RMI）

注意此处的RMI指普通情形中的远程方法调用，不应该与远程方法调用的某些特例(如Java RMI)混淆。

### 请求应答协议

### 远程过程调用

接口定义语言（IDL）

调用语义

幂等操作：反复执行的结果与只执行一次的结果一样。