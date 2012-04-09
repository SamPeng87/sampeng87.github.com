---
layout: post
title: "php也会堵塞，很容易忽略"
description: ""
category: 程序语言
tags: [apache, nginx]
---
{% include JB/setup %}

##前因

以前没注意过php堵塞问题，一直认为php都是马上执行的。今天在PHPChina上面看到有人问：上传的时候如果太慢，是否会堵塞。正好想起来，我去年去百度面试被问起apache和nginx最大区别是什么。我当时没有回答出来，百度面试官告诉我是nginx是异步的，apache是同步的。我当时只记住这个结论。但是没有理解介个问题。

接着，thinksns有一次做了一个项目，是华为北京的一个子公司项目。总出现一旦有人在上传。文件大一点。其他人啥都干不了。我顿时就SB了。几件事联系起来，就可以联系和思考为什么会这样。

##测试

首先要模拟上传过程，浏览器上传，实际还是标准的http协议，那用个表单上传和直接php里面sleep没有本质区别的。其次，测试过程中要区分有session和没有session的情况

1. 无session的时候测试



test1.php

    <?php
    echo "你猜我会不会马上冒出来";

test2.php
    
    <?php
    echo "我要堵塞住整个web服务";
    sleep(30);

先运行test2，然后运行test1。

**结果**: test2在运行的过程中，test1再运行是没问题的，直接输出。非堵塞

2. 有session时测试

test1.php

    <?php
    session_start();
    echo "你猜我会不会马上冒出来";

test2.php
    
    <?php
    session_start();
    echo "我要堵塞住整个web服务";
    sleep(30);

先运行test2，然后运行test1。

**结果**: test2运行过程中，test1无法直接运行。没有输出。堵塞

3. 再试试手动释放session

test1.php

    <?php
    session_start();
    echo "你猜我会不会马上冒出来";

test2.php
    
    <?php
    session_start();
    echo "我要堵塞住整个web服务";
    sleep(30);


先运行test2，然后运行test1。

**结果**: test2运行过程中，test1直接运行。非堵塞

4. 再试试打开两个浏览器session_start()的结果

代码同第2步

**测试过程**

在浏览器A中运行test2，再去浏览器B中运行test1

**结果**: test2运行过程中，test1直接运行。非堵塞

##结论

1. 在session_start的时候，session会话堵塞。在当前浏览器下，当前会话就别想做其他事了。所以，在耗时的过程之前，请session_write_close.
2. 并不会影响到其他会话（也就是浏览器）

> nginx的环境未测试。以上测试建立在apache下


