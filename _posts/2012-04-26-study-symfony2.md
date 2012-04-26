---
layout: post
title: "学Symfony框架笔记2-Controller"
description: "学Symfony框架笔记2-Controller
category: 框架
tags: [Symfony]
---
{% include JB/setup %}

##关键的对象

*Response*


响应对象，任何Action需要返回该对象。render只是一种渲染模板引擎后再生成一个Response对象的封装

##关于参数

**参数是有route匹配后传递给Controller的。并且参数可以是多个，同样由route定义  **

     hello:
          pattern: /hello/{first_name}/{last_name}
          defaults: { _controller: HelloBundle:Hello:index,color:green}

以上就定义了一个hello的route，所有匹配pattern的请求。将传递给HelloController的indexAction3个参数$first_name,$last_name,$color;

注意

+ **参数的个数只能少不能多**
+ **如果有多的参数，多的参数没有在route中定义。那么参数就必须有默认值**
+ **参数的顺序可以打乱**

**参数还可以以对象的方式存在**

任何Action可以使用Request $request做为参数对象。该对象实际是一个请求对象


##通用方法

$this->redirect(XXX) //跳转到某个请求，默认是302。如果想改成301。在第二个参数上添加

$this->forward('BundleName:ControllerName:ActionName',array(参数啦))；请求到另一个地方去。但实际上不是跳转

##关于模板渲染

$this->render是直接返回一个Response对象。如果只是想渲染一个模板:$this->renderView即可

模板引擎不仅仅是用this使用，可以调取服务来使用

     $templating = $this->get('tempting');
     $content = $templating....

##访问其他的服务

$this->get(XXX);

##关于错误和404

**异常的抛出**
throw $this->createNotFoundException("xxxxxx");

直接抛出异常 throw new \Exception('XXXXX').是会直接发一个500http响应头


##关于session

session是在请求中封装的

     $session = $this->getRequest()->getSession();
     $session->set('foo','bar');
     $foo = $session->get('foo');
     $session->setLocale("fr")

**Flash Message**

Symfony2定义了一种页面之间跳转的短消息。也就是A跳到B页面时携带了额外的消息过去。这就是FlashMessage

A页面做逻辑处理时

     $this->get('session')->setFlash("xxx","xxxxxx");

B页面进行逻辑判断

     $session = $this->get('session');
     if($this->session->hasFlash('xxx'))
          $message = $this->session->flash('xxx');

具体响应和请求的对象参看api文档

