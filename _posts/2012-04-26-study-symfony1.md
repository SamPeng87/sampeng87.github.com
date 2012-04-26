---
layout: post
title: "学Symfony框架笔记1-Request"
description: "学Symfony框架笔记1-Request"
category: 框架
tags: [Symfony]
---
{% include JB/setup %}



##Request是针对http协议请求头的一个对象封装

分别具有一下属性

* request:
     请求中$_POST部分的数据
     对应ParameterBag对象

* query:
     请求中$_GET部分的数据
     对应ParameterBag对象

* cookies:
     请求中$_COOKIE部分的数据
     对应ParameterBag对象

* attributes:
     不是http协议的封装。
     对应ParameterBag对象

* files:
     请求中$_FILES部分的数据
     对应FileBag对象

* server:
     请求中$_SERVER部分的数据
     对应ServerBag对象

* headers:
     请求中$_SERVER里面的http协议头数据。如Content-Type
     对应HeaderBag对象


ParameterBag对象的数据操作

* all():返回所有参数
* keys():返回所有的keys
* replace():替换当前的参数为一个新的
* add():添加参数
* get():通过参数名返回参数值
* set():通过参数名设置参数值
* has():是否参数定义
* remove():移除参数

其次还有以下的过滤方式

getAlpha():只取字符串形式的参数

getAlnum(): 取即有字符串又有数字的参数值

getDigits():只取数字类型的参数

getInt():取回参数，并且强制转换成int类型

filter():过滤参数，通过filter_var方法







