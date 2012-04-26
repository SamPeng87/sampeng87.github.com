---
layout: post
title: "学Symfony框架笔记3-Routing"
description: "学Symfony框架笔记4-模板"
category: 框架
tags: [Symfony]
---
{% include JB/setup %}


无非是一堆标签

http://twig.sensiolabs.org/doc/tags/index.html

特色功能filter

http://twig.sensiolabs.org/doc/filters/index.html

异常

http://twig.sensiolabs.org/doc/extensions.html

全局模板变量

+ app.security - 安全的上下文
+ app.user - 当前用户对象
+ app.request - 当前请求对象
+ app.session - 当前session对象
+ app.enviroment - 当前环境是如何
+ app.debug 如果是debug模式这个变量就是真


防止xss攻击

     //模板里面 
     {{ XX|raw }}
     //用php
     $view->escape(xxx);
