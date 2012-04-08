---
layout: post
title: "Mac下如何编译Apache的扩展"
description: ""
category: 解决方案
tags: [Apache, MacOS]
---
{% include JB/setup %}

网上找了一圈都没看到解决方案。最后自己折腾了一下。

首先，是看到编译包里面需要一个apxs命令。那就网上看了一下，这个的apache的devel里面带的。

那就简单了。。看一眼这个文件

里面能看到：

    my $installbuilddir = "/usr/share/httpd/build";

这就是最关键的位置了。

修改apache扩展包里面的MakeFile文件。修改相应的目录到这个目录下。
再编译。就不会有编译找不到软件的情况了

    PS:
    top_srcdir=/usr/share/httpd
    top_builddir=/usr/share/httpd

其他的要找什么mk之类的配置（因为不同的扩展不同的配置参数），都在**build**下面。