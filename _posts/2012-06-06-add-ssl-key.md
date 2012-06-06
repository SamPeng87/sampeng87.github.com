---
layout: post
title: "添加JAVA的SSL认证文件"
description: "添加JAVA的SSL认证文件"
category: 解决方案
tags: [ssl, kentool, java]
---
{% include JB/setup %}

###生成key
    
    keytool -genkey -alias jboss -keyalg RSA -keystore ./xxx.keystore

其他见--help，中文帮助

###导出key
    
    keytool -export -alias jboss -file server.crt -keystore ./xxx.keystore 

###导入到系统ssl中
    
    keytool -import -file server.crt -keystore /System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home/lib/security/cacerts -alias jboss

默认密码

    changeit

这个默认密码很搞笑 XD
