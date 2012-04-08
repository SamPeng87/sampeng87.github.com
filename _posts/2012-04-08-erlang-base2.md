---
layout: post
title: " erlang学习笔记二---进程管理"
description: ""
category: 程序语言
tags: [Erlang]
---
{% include JB/setup %}

恩。。最近2个月又很多事情要忙。so，一直没时间来学习erlang。。这两天稍微空闲一点了。就继续学习erlang语言吧！nice~
 
前面基础已经搞定了，虽然2个月没碰。但是学过的东西是不会那么容易忘记的。
现在开始直接学习erlang的进程管理，其实2个月前这部分学过。。只是一直没做笔记就忘记了。。下面就一边做笔记一边回顾

##erlang进程管理基础
erlang的进程管理确实很简单。3部走：

* spawn（参数列表）：创建一个进程，并执行参数列表中的Fn

* Pid ！message ：向Pid发送一个消息。message可以为erlang所认定的变量和list以及任意表达式。

* receive：最后开始监听，本程序运行的进程，接受到了哪些响应。然后根据这些响应做出相应的动作

 
看起来是很简单的3部。。运用起来最经典的pingPong测试能够很好的反应出这3点所起到的作用。

所谓pingPong测试。就是像打乒乓球一样，A向B发送请求，B马上向A回应，A再发送请求，直到A发送的请求为finished动作。

下面是代码:

{% highlight erlang %}
-module(pingPong).  
-export([start/0,ping/2,pong/0]).  
ping(0,PongPID)->  
    PongPID ! finishied,  
    io:format("ping finished~n",[]);  
ping(N,PongPID) ->  
    PongPID ! {ping,self()},  
    receive  
    pong ->  
        io:format("Ping receive pong~n",[])  
    end,  
    ping(N-1,PongPID).  
pong()->  
    receive  
    finished ->  
        io:format("Pong finished~n",[]);  
    {ping,Ping_PID} ->  
        io:format("Pong receive ping ~n",[]),  
        Ping_PID ! pong,  
        pong()  
    end.  
start()->  
    Pong_PID = spawn(pingPong,pong,[]),  
    spawn(pingPong,ping,[3,Pong_PID]). 
{% endhighlight %}

这里ping3次。。测试结果

    Pong receive ping
    <0.117.0>
    Ping receive pong
    (emacs@peak-tang-PC)23> Pong receive ping
    (emacs@peak-tang-PC)23> Ping receive pong
    (emacs@peak-tang-PC)23> Pong receive ping
    (emacs@peak-tang-PC)23> Ping receive pong
    (emacs@peak-tang-PC)23> ping finishied
    (emacs@peak-tang-PC)23> Pong finished

**注意**：多进程是异步处理。也就是不一定要执行receive然后再执行下面的。。receive只是告诉该进程，我需要做一个监听，看看被传递进来什么动作

##进程管理的细节

1. erlang进程管理有一个C/S的处理方式，它所谓的C/S并不是两个完整的程序，而是两个进程，一个进程作为service，一个（其他）进程作为client。
 
就像一个房子里好多的人，一个人是提供其他人食物的，其他人肚子饿的时候只需要对那一个人说：我饿了，给我食物。那个人就给发出这个请求的人一份食品。。
 
ok，这里就有一个疑问：作为一个进程，怎么区分service和client?
 
拿上面那个例子来说明，
 
    PongPID ! {ping,self()}
    这里就宣告了我是客户端，PongPID是服务端。self()是自身进程的Id.
    相对应的。。。如果是看源码。。最明显的是这里
    pong（） 中的 {ping,Ping_PID} 模式匹配
    这也是宣告了我自身是服务端，我所监听的对象，必须返回给我两个参数。一个是pong，一个是客户端的进程id。。
 
那也就是说上面的代码是符合C/S模式的。

作为服务端的ping进程，会监听发送给自己的进程消息。如果有一个是{ping,PID}, 就告诉pid我收到了你的信息。

作为客户端的pong进程，监听所获得的响应，如果是服务端发回的响应，就做出响应的处理。这里是做了一个循环。继续向服务端发送请求。

2. 上面这个问题就会引起另一个问题，如果超时咋办？

**用receive的after关键字**

语法如下：

    receive
    Pattern1 [when Guard1] ->
    Expressions1;
    Pattern2 [when Guard2] ->
    Expressions2;
    ...
    after Time ->
    Expressions
    end
 
after激活的条件就是达到Time（毫秒级别）后，后面的Expressions将开始执行。
 
书上讲的常用例子。。这东西不能讲得太细- -。

只是说明一下after后面有两个特殊属性：

+ 0（无穷小）,即没有超时，马上执行后面的表达式
+ infinity（无穷大），即永久超时。该进程挂起

还是在上面pingpong测试里面来体现一个综合的应用

将上面例子的pong方法做以下修改

{% highlight erlang %}
pong(T) ->  
    receive   
    finished ->  
        io:format("Pong finished~n",[]),  
        true  
    after T ->  
     receive   
         {ping,Ping_Pid} ->  
         io:format("Pong receive ping~n",[]),  
         Ping_Pid!pong,  
         pong(T)  
         end  
    end.  
{% endhighlight %}

修改一下start中的参数

    Pong_PID = spawn(pingPong,pong,[3000])

运行
 
    (emacs@peak-tang-PC)34> pingpong2:start().
    <0.164.0>
    (emacs@peak-tang-PC)35> Pong receive ping
    (emacs@peak-tang-PC)35> Ping receive pong
    (emacs@peak-tang-PC)35> Pong receive ping
    (emacs@peak-tang-PC)35> Ping receive pong
    (emacs@peak-tang-PC)35> Pong receive ping
    (emacs@peak-tang-PC)35> Ping receive pong
    (emacs@peak-tang-PC)35> ping finishied
    (emacs@peak-tang-PC)35> Pong finished
 
结果和以上结果类似。。。。只是每个pong-ping循环是每隔3秒出现的结果

3. 关于进程的注册

erlang在进程管理的方便上，必然导致在一个比较大的系统里面到处充满了全局进程以及一些私有进程。
那如何管理这些进程呢？spawn是能够返回一个进程id。那这个id能否在储存起来？
讲真正的进程id隐藏，并且将其id绑定到一个atom上。这个过程就叫注册进程
 
在注册进程方面有以下4种BIFs
 
* register(AnAtom,Pid)
    
    注册某一个进程的Pid绑定到AnAtom上，当然，老规矩。atom不能是使用过的字符串。否者会出错

* unregister(AnAtom)
    
    取消AnAtom上绑定的pid进程号。再次使用该AnAtom，将再没有当做进程id使用的效果

* whereis(AnAtom)->Pid | undefined
    
    判断一个AnAtom是否已经被注册。如果注册，则返回Pid，反之，返回一个atom undefined。

* registered() -> [AnAtom:atom()]
    
    返回在系统中所有已经注册的进程列表

将上面的乒乓测试中的start方法进行修改

{% highlight erlang %}
start()->  
    Pong_PID = spawn(pingpong2,pong,[3000]),  
    register(pong_pid,Pong_PID),  
    spawn(pingpong2,ping,[3,pong_pid]).  
{% endhighlight %}

所得测试结果和以上完全相同


如何通过进程管理，来开发并行项目呢？

下面这个代码块可以很好的诠释

{% highlight erlang %}
-module(ctemplate).  
-compile(export_all).  
start()->                 %%第一步：创建一个进程用来执行loop函数  
    spawn(fun()->  
          loop([])end).  
rpc(Pid,Request)->        %%第二步：对指定服务进程发送请求  
    Pid ! {self(),Request},  
    receive  
    {Pid,Response}-> %%第四步：对回答做出响应  
        io:format("Response ~p~n",[Response])  
    after 3000 ->   %%超时3秒  
     true  
    end.  
loop(X)->  
    receive  
    %%第三步对请求做出回答  
    {Pid,Request} ->  
        Pid ! {self(),"request are received"},  
        io:format("Received:~p~n",[Request]),  
        loop(X)  
    end.  
{% endhighlight %}

需要注意的地方是。就算没有用spawn来做进程的创建，程序本身就要占用一个进程，如rpc这里运行就是在一个进程中运行的。所以rpc的参数Pid是目标进程id，self（）是他本身的id。所以他所监听进程获取消息的时候Pid应该是目标id。服务端需要把自己的id给传回去。
 
该程序是一个基于乒乓现象的编码方式。。服务端发送了请求，客户端一定要有所响应。。如果想弄成最基本的C/S模型，将Pid ! {self(),"request are received"},去掉即可。
 
over。。进程管理就到此为止。erlang对于进程管理来言确实东西并不多。。
