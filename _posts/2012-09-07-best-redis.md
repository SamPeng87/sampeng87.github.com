---
layout: post
title: "Redis源码分析总结报告"
description: ""
category: 深入研究

tags: [redis, c]
---
{% include JB/setup %}

> 最近晚上回去不想打dota了，实在是打伤了。闲暇下来就看会redis源码，以前一直听说redis代码少来着。所以就拿他来开刀了。学习一门新的语言，掌握新的技术和编程思想，最方便快捷的方式就是看别人的源码。看了一周，大概逻辑看明白了。这篇日志前面一段是总结，从源码中得到的使用启发。后一段是一个目录，会分别对一些redis重要和独立的模块进行分析。所以这篇日志会一直更新下去，直到结束浏览redis源码。


# 最优使用redis 

## 提高内存的使用效率

### redis的malloc

redis的内存管理以及大小估算，详细的分析见[内存分析][1]

内存利用率 tcmalloc > jemalloc > libc (通过redis-cli info mem_fragmentation_ratio 查看得知)

内存占用 tcmalloc > jemalloc > libc (通过redis-cli info used_memory_peak_human 查看得知)

占用大小3者还相差不小。tcmalloc占用很多，但是效率就高了很多。在set/get时jemalloc和libmalloc是稳定在60000/s左右。tcmalloc能飚到100000/s左右。所以内存利用率tcmalloc优化后是取胜的。


## 降低没必要的处理

这一块就不知道是我对源码的理解对还是错了。发现有两个地方的使用是可以优化的,实际上我用radis自带的工具是没测出来有什么区别的。我估摸着是一个长时间运行的问题

1. 没必要开启过多的数据库。也就是configure中的database配置不要开太多。源码中会发现每次redis的BIO服务会进行清理每个数据库下的各种数据，如把数据flush到临时文件去等等操作。如果数据库过多，虽然有很大一部分是空的。实际上还是要消耗一定的用户cpu时钟的时间

2. 关于过期的问题

	串过期的处理方式：
	每个请求都调用expireIfNeeded
	这个方法的作用是判断是否有超时时间，如果没有。返回。
	如果有：
	
	1.通知同步到所有的slave机上去，并且准备同步到AOF文件里面
	
	2.删除该key
	
	服务器定期的循环判断。从db中的过期字典中随机取10个成员比较和当前时间的大小看是否过期。过期就删除了。。。。
	
	
	也就是说
	redis有两种机制。
	
	1，主动，每请求某一个key的值的时候将其值进行过期判断
	
	2，被动，定期的去删除最多10个设置了过期时间的key


# Redis模块分析目录

1. [redis的内存分析][1]
2. redis的定时任务
2. redis的BIO
3. redis的VM
4. redis核心数据结构dict
5. redis核心数据结构adlist
6. redis核心数据结构zmap
7. redis核心数据结构skiplist
8. redis核心数据结构ziplist
9. redis的网络封装以及异步IO模型

[1]: http://www.sampeng.org/2012/09/11/redis-mem/
