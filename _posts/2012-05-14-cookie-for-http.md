---
layout: post
title: "http协议深入理解-cookie篇"
description: "cookie,http协议"
category: 深入研究
tags: [cookie, http]
---
{% include JB/setup %}

http是现代互联网最有价值的协议，搞web开发3年了，居然只是了解个皮毛，我表示压力很大。所以接下来的的日子里，深入的理解和了解整个http协议涉及到的相关部分。

这一篇是说cookie骗

主要涉及的RFC文件如下

* [RFC2109][2109]

    基础协议标准,扩展的还有很多，比如2616，2965。但都是基于2109扩展过来的。


* [RFC6265][6265]
    
    现在正在使用的协议标准**6265**

通常而言cookie就和语境一样的去设置就行了，但标准有几个地方需要注意，但有容易忽略的地方。

+ domain域的问题
    
    同时path=/,domain=".sample.com"

        访问域名 请求头中的域 最后结果 

        sample.com          1个 456 

        bbs.sample.com      1个 456 

        test.bbs.sample.com 1个 456 

    sample.com设置.sampeng.com

    bbs.sample.com设置.bbs.sampeng.com

        访问域名 请求头中的域 最后结果 

        sample.com          1个 123 

        bbs.sample.com      2个 123 

        test.bbs.sample.com 2个 123 

    和上面的测试相同，但是顺序不同

        访问域名 请求头中的域 最后结果 

        sample.com          1个 456

        bbs.sample.com      2个 456

        test.bbs.sample.com 2个 456

    **结论:优先得到第一个种的。如这里就会是456.因为cookie的相应头是2个都带回来了。php只拿第一个**

    反过来试试

    sample.com      设置.bbs.sample.com

    bbs.sample.com  设置.sample.com

        访问域名 请求头中的域 最后结果 

        sample.com          1个 456 

        bbs.sample.com      1个 456 

        test.bbs.sample.com 1个 456 

    直接在sample.com设置.bbs.sample.com

    不让bbs.sample.com发送set-cookie

        访问域名     请求头中的域 最后结果 

        sample.com            0 null 

        bbs.sample.com        0 null 

        test.bbs.sample.com   0 null 


    **结论：设置非本请求访问的时候只能设置根域名下的。**


    再同时设置子域名呢？

    只有bbs.sample.com中设置的有效。。

        访问域名 请求头中的域 最后结果 

        sample.com          null null 

        bbs.sample.com      1个 456 

        test.bbs.sample.com 1个 456 

    结论：
    + 直接设置根域，全部子域共享cookie
    + 各设各的，访问子域时优先得到第一个set的cookie。
    + 子域设的值无法在主域下得到。见最后一条测试

    
+ cookie值的问题
    
    值尽可能的urlencode。防止特殊字符干扰整个value值。如空格和分号

    整个值到分号之前不要有空格，理论上浏览器客户端会给trim掉。但如果对方客户端是自己写的，有可能不会这么做。所以整个值到分号之前都不要有空格，如果必须有，urlencode。RFC文档里面也说明了这一点规范。

        Remove any leading or trailing WSP characters from the attribute-name string and the attribute-value string.
 
        WSP的意思是whitespace
         
        在RFC5234中定义的WSP是这样的
         
        WSP            =  SP / HTAB
                          ; white space
        SP             =  %x20
        HTAB           =  %x09
                          ; horizontal tab
 

+ 多条set-cookie

    服务端不要再一条set-cookie头里面设置多个值，因为,符号会干扰整个语义。一个set-cookie就是一条cookie.见[RFC6265][6265]

       Origin servers SHOULD NOT fold multiple Set-Cookie header fields into

       a single header field.  The usual mechanism for folding HTTP headers

       fields (i.e., as defined in [RFC2616]) might change the semantics of

       the Set-Cookie header field because the %x2C (",") character is used    

       by Set-Cookie in a way that conflicts with such folding.

+ 关于cookie的长度
    
    [RFC6265][6265]中如下定义

        Practical user agent implementations have limits on the number and
        size of cookies that they can store.  General-use user agents SHOULD
        provide each of the following minimum capabilities:

        o  At least 4096 bytes per cookie (as measured by the sum of the
           length of the cookie's name, value, and attributes).

        o  At least 50 cookies per domain.

        o  At least 3000 cookies total.

        Servers SHOULD use as few and as small cookies as possible to avoid
        reaching these implementation limits and to minimize network
        bandwidth due to the Cookie header being included in every request.

        Servers SHOULD gracefully degrade if the user agent fails to return
        one or more cookies in the Cookie header because the user agent might
        evict any cookie at any time on orders from the user.

    再看看最基础的[RFC2109][2109]

        Practical user agent implementations have limits on the number and
        size of cookies that they can store.  In general, user agents' cookie
        support should have no fixed limits.  They should strive to store as
        many frequently-used cookies as possible.  Furthermore, general-use
        user agents should provide each of the following minimum capabilities
        individually, although not necessarily simultaneously:

        * at least 300 cookies

        * at least 4096 bytes per cookie (as measured by the size of the
        characters that comprise the cookie non-terminal in the syntax
        description of the Set-Cookie header)

        * at least 20 cookies per unique host or domain name

        User agents created for specific purposes or for limited-capacity
        devices should provide at least 20 cookies of 4096 bytes, to ensure
        that the user can interact with a session-based origin server.

        The information in a Set-Cookie response header must be retained in
        its entirety.  If for some reason there is inadequate space to store
        the cookie, it must be discarded, not truncated.

        Applications should use as few and as small cookies as possible, and
        they should cope gracefully with the loss of a cookie.

    每个cookie的大小不变，都是4096

    cookie的个数在现在大多数浏览器支持的增多

    了解这个信息是有好处的，首先是cookie可以存多少数据做到心中有数。其次，安全性。因为这个特性。完全可以通过构造无效字符来增大cookie的长度。然后利用客户端自动截取的特性来篡改cookie值。


如果用php发送cookie的头，最好直接用setcookie函数，不要手动的header发出去。setcookie将会遵循[RFC6265][6265]发送标准的cookie头。而且是每调用一次就会发一条。

有个tips，看了源码才知道的。如果用setcookie，value值不需要进行urlencode。源码里面有这么一句

    encoded_value = php_url_encode(value, value_len, &encoded_value_len);

setcookie会自动的urlencode

**重视和深入理解http协议可以理解很多请求过程中的陷阱以及安全问题。所以查看RFC文档是了解这些标准的最佳途径。不仅要看，更多的是要进行测试。毕竟RFC文档些写的有点枯燥**



[2109]: http://tools.ietf.org/html/rfc2109
[6265]: http://tools.ietf.org/html/rfc6265