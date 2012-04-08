---
layout: post
title: "php for man,php的命令行手册"
description: ""
category: 程序语言
tags: [php]
---
{% include JB/setup %}

man手册绝对是linux下最好用的帮助手册。chm神马的都是浮云。

我躺床上突然想php是不是有。果然，2011.06.25的时候php发布了pman。就是php的man手册工具。

安装起来还是比较简单的。

假设已经安装了pear,如果没有，[看这里(for mac,其他请google之)][1]

    pear install doc.php.net/pman

然后就可以使用了

    pman strlen

接着才是安装这个最主要的目的，如果用其他的ide，比如Zend之类的。安装这玩意和不装没什么区别。因为我在重新使用vim。vim的K键就能直接查当前字符串的man手册。结合起来才是最完美的使用，修改./.vimrc

    "设置pman手册
    autocmd FileType php setlocal keywordprg=pman

再随便打开一个php文件，然后找一个函数，按K，喔，搞定。收工。

[1]: http://clickontyler.com/blog/2008/01/how-to-install-pear-in-mac-os-x-leopard/
