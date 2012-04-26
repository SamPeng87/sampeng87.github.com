---
layout: post
title: "学Symfony框架笔记5-常用命令集合"
description: "学Symfony框架笔记5-常用命令集合"
category: 框架
tags: [Symfony]
---
{% include JB/setup %}

首先所有列表可以直接输入 
    
    php app/console

但还是抽出几个常用的，我把这些都配了缩写。。懒得写一堆东西啊

调试router

    php app/console router:debug [router_name]

生成Bundle

    php app/console generate:bundle --namespace=XXX/XXXBundle

生成数据库ORM

    php app/console doctrine:generate:entity --entity="SamPengStoreBundle:Test" --fields="name:string(255) price:float description:text"


给ORM对象添加get和set对象

    php app/console doctrine:generate:entities XXX/XXXBundle/Entity/XXX

创建数据库结构

    php app/console doctrine:schema:update --force

从数据库获取表结构并生成配置文件
    
    php app/console doctrine:mapping:import bundle yml