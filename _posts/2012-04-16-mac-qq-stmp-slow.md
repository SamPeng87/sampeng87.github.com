---
layout: post
title: "解决qq邮箱的smtp服务在mail下慢的方法"
description: ""
category: 工具癖
tags: [qq邮箱, smtp]
---
{% include JB/setup %}

用了几个Mac下的mail的工具。都是各种不适应，主要是和系统的集成性太差。

后来用了自带的mail，顿时觉得好用多了。有很多吸引我的集成

+ 集成OmniFocus
+ 集成evernote
+ 集成iCal
+ 集成联系人

这就方便多了，但是一直有个事让我很苦逼

发邮件要等1分钟！！！！

于是我观察了一下测试邮箱服务连接时候的信息，原来Mac自带的mail在解析smtp服务的时候是每个默认端口都跑一遍。。这个太尴尬了。果断指定成25。

我要的瞬发效果就出来了。

问题解决。