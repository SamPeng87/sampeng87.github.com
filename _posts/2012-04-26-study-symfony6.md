---
layout: post
title: "学Symfony框架笔记6-单元测试"
description: "学Symfony框架笔记6-单元测试"
category: 框架
tags: [Symfony]
---
{% include JB/setup %}

在一个目录下执行全部的单元测试

    phpunit -c app src/XXX/XXX/Tests/DirectoryName/

运行制定的测试用例
    
    phpunit -c app src/XXX/XXX/Tests/XXX/XXX.php

运行所有的单元测试

    phpunit -c app src/Namespace/BundleName


其他其实没什么了，特色是集成了Symfony的各种页面和客户端的测试方式。这些是其他框架没有做到的。这些等我要做这种controller测试的时候再说吧。。
