---
layout: post
title: "初识WebSocket"
description: "记录下自己学习WebSocket相关的知识点，以及做一个基于WebSocket的简单的BBS系统的过程等"
date: 2018-01-24
tags: [Java, WebSocket, HTML5]
categories: [Tech]
---

> 刷技术站点的时候，偶然间看到 WebSocket 的简介，觉得是个不错的技术，便想花费一点时间来学习一下。在此把学习的资料和过程简单记录一下，以便日后自己回顾，如果能给其他人一点帮助或者启发也未尝不是一件好事。在学习过程中，自己尝试使用WebSocket做点小东西结合理论知识加深对WebSocket的理解。

### Contents
* [What is WebSocket][what-is-websocket-contents-link]
* [Reference][reference-contents-link]

---

[what-is-websocket-contents-link]: #1-what-is-websocket
[reference-contents-link]: #3-reference


### 1. What is WebSocket
> WebSocket是一种在单个TCP连接上进行全双工通讯的协议。WebSocket通信协议于2011年被IETF定为标准RFC 6455，并由RFC7936补充规范。WebSocket API也被W3C定为标准。  —— wikipedia

以上是来自wikipedia的词条信息。词条中关于WebSocket是什么的描述很精简，其中有这么几个关键词：`TCP连接`，`全双工`，`通讯的协议`，`标准`。

首先从理解上边这几个关键词开始。
* `通讯的协议`

    首先是理解这个`协议`，WebSocket是一个用于`客户端`与`服务器`之间通信的协议(这里的客户端一般为浏览器)。这样一来，就有个问题，WebSocket和如今常用的浏览器与服务器间通信的`HTTP/HTTPS`协议有什么关系？之间又有什么异同呢？

    那先来了解下WebSocket与HTTP/HTTPS之间有什么关系。下图是WebSocket的工作原理图(在此仅用于展示WebSocket与HTTP之间的关系，具体工作原理稍后再做较详细说明)，整张图表示一个完整的 WebSocket 通信过程。其中包括浏览器与服务器之间的握手过程，握手成功之后的双向多次通信，以及不再需要通信而由一方关闭连接或者由于异常情况导致的连接中断。其中红色箭头线为HTTP协议的请求与响应。那么可以通过这张图了解到，*`WebSocket协议中使用了HTTP协议来处理握手步骤`*。

    ![WebSocket与HTTP间的关系][websocket-img-1]

    那么WebSocket和HTTP之间有什么异同呢？其实两个协议之间，必然有很多协议定义及实现上的细节性差别。这里想要通过比较来了解的异同，是作为一个使用者，从使用角度来看待它们之间的异同。先从相同点来看。
    * 两者都是用于`浏览器`与`服务器`之间通信的协议(这里只是从常用角度来看，当然服务器端也可以发起HTTP请求等)
    * 都是建立在TCP协议之上(这是从OSI模型角度来看，HTTP和WebSocket都应用层的协议，所以需要底层的网络协议来支撑)
    
    再看看两者的不同点。
    * HTTP为单向协议，也就是只能由客户端发请求数据给服务端，然后服务端针对这次请求数据返回响应数据。而不能由服务端发起请求数据/响应数据到客户端。而WebSocket最大的特点就是客户端与服务端通过握手建立连接之后，双发都可以互发数据。
    * 协议标识符的区别，HTTP的协议标识为`http://`(如果加密则为`https://`)，而WebSocket的协议标识为`ws://`(如果加密则为`wss://`)。下图或许可以更为直观的表示它们之间的区别

    ![WebSocket与HTTP间协议标识区别][websocket-img-2]


* `标准`
* `TCP连接`

    该关键字所描述的就是WebSocket是建立在TCP协议的基础上的。

* `全双工`
    
    怎么理解全双工呢？还是先用wikipedia上词条的解释来做个铺垫。
    >全双工（full-duplex）的系统允许二台设备间同时进行双向数据传输。一般的电话、手机就是全双工的系统，因为在讲话时同时也可以听到对方的声音。
    全双工的系统可以用一般的双向车道形容。两个方向的车辆因使用不同的车道，因此不会互相影响。

    和其他的通讯协议做个比较或许更便于理解，那和HTTP协议做个简单的比较说明。
    HTTP属于半双工（half-duplex）的协议，半双工的系统允许二台设备之间的双向数据传输，但不能同时进行。因此同一时间只允许一设备发送数据，若另一设备要发送数据，需等原来发送数据的设备发送完成后再处理。结合实际的HTTP协议应用来说，该半双工系统的两个设备就是`浏览器`和`HTTP服务器`，一个HTTP通讯过程大致为`浏览器`发起一个请求，`HTTP服务器`接收到这个请求之后，经过一定程度的业务处理，再返回一个返回信息给`浏览器`（忽略了HTTP握手过程）。
    ![使用多个连接和使用持久链接的对比][websocket-img-3]

---
[websocket-img-1]: {{ site.url }}/assets/posts_img/websocket/WebSockets-Diagram.png
[websocket-img-2]: {{ site.url }}/assets/posts_img/websocket/HTTP_WebSocket_compare.jpg
[websocket-img-3]: {{ site.url }}/assets/posts_img/websocket/HTTP_persistent_connection.svg


### 3. Reference

* [wikipedia关于WebSocket词条的介绍(请科学上网)][reference-link-1]
* [@Ovear关于知乎“WebSocket 是什么原理？为什么可以实现持久连接？”的回答][reference-link-2]
* [java WebSocket开发入门WebSocket][reference-link-3]
* [WebSocket介绍][reference-link-4]
* [WebSocket和Socket的区别][reference-link-5]

---

[reference-link-1]: https://zh.wikipedia.org/wiki/WebSocket
[reference-link-2]: https://www.zhihu.com/question/20215561/answer/40316953
[reference-link-3]: https://www.jianshu.com/p/d79bf8174196
[reference-link-4]: https://hpbn.co/websocket/
[reference-link-5]: https://www.jianshu.com/p/59b5594ffbb0


