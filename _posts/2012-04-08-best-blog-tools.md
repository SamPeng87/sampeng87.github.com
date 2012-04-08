---
layout: post
title: "最好的写博客方式诞生"
description: "github,markdwon,jekyll,git,blog"
category: 工具癖
tags: [github, markdown, jekyll,git]
---
{% include JB/setup %}

##废话

写博客应该是一件很开心的事，我以前也写很多博客，但后来因为个人懒惰了。就没写了。我不写博客的主要原因就是我懒得进后台去写博客

现在github有了牛b的pages。免费的哦。速度也不错。最最最关键的，是一种支持markdown的工作方式.

周末没什么事做，就把博客搭建起来。等会把其他站的博客全迁移过来.

为什么不用

##初始化

1. 你要有个github账号。就算不懂英语[这个wiki][1]写的很清楚了

2. 安装[Jekyll][2]

3. 安装[Jekyll Bootstrap][3]

4. 创建git资源

注意，创建的git资源直接名字就为XXXX.github.com吧。这样就自动的成为一个github的page资源了。

5. jekyll你要知道的

_config.yml
    
    只要加这几句就行了。其他的默认的不用黏贴进来咯
    auto: true
    pygments: true
    markdown: rdiscount

启动服务
    
    进入日志目录
    jekyll --service --pygments

目录关系
    
    _includes {% include xxx %} 就是到这里去找东西了
    _layouts 布局，模板文件
    _plugins 插件
    _posts markdown文件是在这里写的
    _site 要用gitignors过滤掉，这个目录是不需要提交到github上的。本地预览就输出到这里了


##绑定域名

* 在repos下建立一个CNAME文件

CNAME的作用是把让github将域名指定到username.github.com
     
    echo "sampeng.org" > CNAME

* 指定DNS转发

DNS要知道域名往那边跳
    
    进入DNS管理页面
    a) 如果是顶级域名绑定，添加A记录到207.97.227.245
    b) 如果是二级/N级域名绑定，添加CNAME记录到username.github.com

* 访问绑定的域名搞定收工


##开始写博客

[参看这个页面先][4]

基本流程最后变成

1. 自动生成一个日志

装JB的主要原因就是有个rb工具，懒得去记格式和写文件名

    进入日志所在的目录
    rake post title="xxxxxx"
    最后输出会告诉你文件在哪里

2. 用任何编辑器编写这个文件。保存

3. 本地先预览一下（如果看了上面那些链接，都会告诉你默认路径在哪里，恩，通常是http://localhost:4000）

5. 提交到github

最后提交上去，就像git的使用方式一样，只是可能要等一会自动生成页面

    git add .
    git commit -m "new blog"
    git push -u origin master

相当的顺畅，比进浏览器后台写日志方便多了。

不再用在写博客的时候关心布局乱七八糟的事情咯。写就是了，更关键的是，可以在手机上，任何终端上写好。回头按格式提交到github上就可以了。


[1]: http://help.github.com/mac-set-up-git/
[2]: https://github.com/mojombo/jekyll/wiki/Install
[3]: http://jekyllbootstrap.com/index.html#start-now
[4]: http://jekyllbootstrap.com/usage/jekyll-quick-start.html
