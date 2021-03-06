---
layout: post
title: "Erlang学习笔记一 -----10分钟的基础"
description: ""
category: 程序语言
tags: [Erlang]
---
{% include JB/setup %}

找了几个书店。都没有《Erlang程序设计》这本书。豆瓣上面很华丽的很都人都在求购2手的。既然悲剧了，就凑合着金山词霸慢慢看吧。
 
====================虽然上面没几句，华丽的我还是要分隔出来======================
 
前面几章介绍啊，感谢什么的全部略过。网上无数介绍erlang的优点了。再委屈自己去看一次英文实在是费神。
 
刚进入erlang。就不需要用编辑器。
最简单的那个shell可以了解一下最基础的知识。
 
Erlang的基础语法很简单。

##变量。
和其他的变量有区别的是，必须是首字符大写。然后就是只能一次赋值。

书里面讲得非常详细了。erlang的变量不是真正意义上的变量。因为他不能“变”啊。嘿嘿。我是这样理解的：
 
1. 这个和erlang的特性是有相关性的。erlang的主要工作环境是并行的，可变的变量。可能就会导致并行环境下变量的值琢磨不透。导致程序出现bug的几率就会增加。我比较喜欢这个特性。。我个人觉得变量不变，唯一不好的就是。你必须得为每一个值都想一个代号。有得就有失吧。

2. erlang的=号并不是赋值操作符。而是模式匹配操作符。他最大的作用是把右边的表达式bound到右边去。用作者的话来说。变量只是一个盒子。在第一次等号的时候。可以是任何表达式。第二次等号的时候除非等号右边的值等于第一次等号右边的值。否则模式匹配失败，报错。

如

    A=1;
    没问题。 A与1匹配成功。
    B=2;
    C=1;
    A=B;
    先尝试B匹配1。失败。所以A不能与B匹配
    A=C;
    先尝试C匹配1。成功。所以A能与C匹配（我的理解是，这两个字母都是同一个盒子。）
 
哦，作者把变量比做鸽子的笼子。实在很形象。进去了就别想出来。而且只能进去一个。
 
##变量可以拥有的值。

**数字**

这个由于我是搞php的。实在好理解。erlang对于数值没有做强类型的要求。也没有数值大小溢出的危险。啊。这个真是好- -。学C++的时候生怕哪里溢出了。

    1+1=2
    1/3=0.33333333
    4/2 = 2.0000000

上面是随手写的。小数点后面精确值我也没仔细数
 
##原子和String

这两个书上是分开讲的。。中间插了一个list

书上是说String是一个整数存放的list结构。只是能够打印出来。换句话说，我觉得还是像C一样。字符串实际是以数组的形式存放的。但我认为这样说很难和原子区分开。因为两个的表述形式很相似

原子是'或者啥都不带的字符串。要求首字母为小写

String是“号扩起来的一串。

做php做多了。这3个形式在我眼里都是一样的。但稍微测试一下，再比对一下两者的概念。就很清晰了。

    原子，是不能再分割的。
    也就是说
    A=abc
    [A1|B1] = A
    是报错。因为abc不能分割。所以abc中没有任何东西可以和A1做匹配。后面的操作就不用做了。
    A=“abc”
    [A1|B1] = A
    就是正确的。因为A实际是一窜整数存放的list。这个操作就能取出a字符串的码出来
 
以上基本就是Erlang的基础语法。。。看完这一章，我想我会喜欢上erlang的简单。。
 
 
 