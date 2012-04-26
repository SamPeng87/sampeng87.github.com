---
layout: post
title: "学Symfony框架笔记3-Routing"
description: "学Symfony框架笔记3-Routing"
category: 框架
tags: [Symfony]
---
{% include JB/setup %}

##映射到Action

blog_show:
     pattern: /blog/{slug}
     defaults: { _controller:SamBlogBundle:Blog:show}

就是把任何/blog/xxx的xxx作为参数映射到SamBlogBundle的BlogController中ShowAction中。

##如何把路由配置改到其他地方去

app/config/config.yml

framework:
     router: {resource:"%kernel.root_dir%/config/routing.yml"}


##自定义路由规则

**路由的默认值**

    pattern:  /sampeng/{page}
    defaults: { _controller: SamPengRouterBundle:Default:index}

这个时候访问GET /sampeng就会是一个404错误页

     但是如果
    pattern:  /sampeng/{page}
    defaults: { _controller: SamPengRouterBundle:Default:index,page:1}
    
这个时候访问GET /sampeng就会是一个正常页面。page默认值为1

**路由的规则**

其实这个规则讲的是pattern中间匹配成参数部分（如/sampeng/{page},规则指{page}这部分的规则）。

格式 

        ....
        requirements:
             参数名: 正则或者特殊的规则

* 正则如\d+之类的。
* 特殊的规则如page:1|2.page只有匹配1,2的时候才匹配上这条router
* pattern中如果使用了_format作为匹配名。那就直接匹配的Content-Type上的值

**特殊的路由规则**

* _controller:如果匹配成功，则是指明哪一个controller执行
* _format:匹配请求的格式
* _locale:在session中设置的local


**在router中引入外部的router**

sampeng_hello:
     resource: "@Bundle/Resources/config/routing.yml"
     prefix: /admin

**调试路由**
php app/console router:debug [router_name]

**通过路由创建链接**

$url = $this->get("router")->generate('blog_show',array("page"=>1));

**在模板中创建链接""

相对的url（如/blog/1）
     {{ path('blog_show',{ 'page': 1 }) }}


完整的url(如http://www.example.com/blog/1)
     {{ url('blog_show',{ 'page': 1 }) }} 

