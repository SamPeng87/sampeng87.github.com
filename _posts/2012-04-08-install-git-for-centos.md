---
layout: post
title: "centos搭建Git分布式管理控制系统"
description: ""
category: 版本管理
tags: [git, centos]
---
{% include JB/setup %}

###安装GIT依赖的包

    yum install zlib-devel 
    yum install openssl-devel 
    yum install perl 
    yum install cpio 
    yum install expat-devel 
    yum install gettext-devel

    yum install autoconf

###安装cURL 

    # wget http://curl.haxx.se/download/curl-7.17.1.tar.gz 
    # tar zxvf curl-7.17.1.tar.gz  //解压

    #cd curl-7.17.1 
    # ./configure --prefix=/usr/local/curl 
    # make 
    # make install


###修改ld.so.conf 添加/usr/local/lib

    vi /etc/ld.so.conf 添加/usr/local/lib

###运行idconfig

    /sbin/ldconfig

###安装Git

    wget http://www.codemonkey.org.uk/projects/git-snapshots/git/git-latest.tar.gz 
    tar xzvf git-latest.tar.gz 
    cd git-2011-10-08

    autoconf

    ./configure --with-curl=/usr/local/curl 
    make 
    make install



###测试

终端输入git

![git参数](http://images.cnblogs.com/cnblogs_com/phphuaibei/201110/201110081814067963.png)

相关参数和说明还是很全的

转至http://www.cnblogs.com/phphuaibei/archive/2011/10/08/2202382.html

