---
layout: post
title: "设计之殇"
description: ""
category: 
tags: []
---
{% include JB/setup %}

从一开始接触php，以及程序的时候。我就开始关注程序设计这个概念。只是，当时年少轻狂，只是觉得这东西好玩。就没有往深的方向想。当时入门是看的设计模式。当然这对我来说是非常好的帮助。在这么两年里面一直影响着我对编码的感受以及对代码的把握能力。

两年来，一直在为Thinksns这个产品奋斗着。我喜欢这个领域，它能给更多人带来帮助。但是，我想说的是，设计上，欠缺太多。

    没做过真正的数据库设计，更不用说ER图了。每次一个新功能的引入，都是靠经验直接创建表格

    没有做过任何需求分析，用例分析就更不用说了

    没有做过任何的概要设计

    没有做过任何的集成测试。测试完全靠手点

虽然，在一开始。我是赞同这种行为的，我也觉得设计是为了产品质量的。但产品生产的速度也不容忽视。但是，现在很多问题正在慢慢暴露出来

* 新人融合需要老人手把手的带，并且新人对整个产品的理解和使用只能自己去看代码

* 当需求扩展时，很大程度上是要重写代码，而不是复用。面向对象的封装和复用功能完全没有起到作用

* 动一处而引起多处地方出现bug的问题越来越严重。除了对整个系统熟悉的人，很难做到改动而不影响到其他地方。因为这一切都是没有文档和设计稿的。

* 时间长后，对业务的流转就忘记了。需要翻看代码重新理顺没条逻辑。

* svn的分支功能很难起到做用。几乎处于瘫痪状态。如果是多人协同开发，在不同的分支上面。然后合并起来必然会引起一堆的冲突

而在编码前做好设计工作，这些问题是必然可以避免的。软件设计的主要中心思想就是通过一系列得模型来描述整套系统的运作。而脱离语言的限制。软件架构设计也是从一个最高的“上帝”角度去观察整个业务系统。而如果没有设计，就只能依赖每个程序员的记忆力和抽象把握能力。每次的编程工作，大脑里的流程和景象必须要求是正确无误。最少结构上没有任何问题。如果出现结构性问题。整个系统立马瘫痪。所以，我个人认为，在一个必然要求有业务扩展的产品中。是需要引入详细的程序设计理念，不管这个产品是什么语言编写的。设计过程，不应该忽视掉。

如果是基于产品做的二次开发。产品本身就具备设计文档的时候，就可以省略到架构设计过程，但还是需要有用例分析和主要场景的建模。粒度要能够直接达到编码的程度。

如果是大型项目，也需要有详细的设计开发流程。保证二次开发的结果不会引入新的问题，并且能够高质量的完成客户的需求。

基于以上，我一定得坚持建设起整个产品的设计性和良好的架构拓展能力。这是我最重要的能力成长，也是一种共赢的局面