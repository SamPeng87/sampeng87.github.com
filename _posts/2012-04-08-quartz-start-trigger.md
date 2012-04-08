---
layout: post
title: "文档写不好的产品伤不起啊---quartz的使用"
description: ""
category: 程序语言
tags: [quartz, java]
---
{% include JB/setup %}

公司要做一个底层服务。需要用到定时任务，找了一圈。发现quartz很好很强大。也很简单。网上很多教程。就不多重复了。

只是有一些东西网上教程没有，文档也没写清楚。真是浪费我不少时间啊。都是针对1.8的。最新版已经是2.1了亲

获取调度下的触发器并全部启动正确的写法

{% highlight java %}
triggerGroups = sched.getTriggerGroupNames();
       for (int i = 0; i < triggerGroups.size(); i++) {
           triggers = sched.getTriggerKeys(GroupMatcher.triggerGroupContains(triggerGroups.get(i)));
          for(TriggerKey trigger:triggers){
              sched.rescheduleJob(trigger,sched.getTrigger(trigger));
          }
       }
{% endhighlight %}
