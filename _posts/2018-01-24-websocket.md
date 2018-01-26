---
layout: post
title: "初识 WebSocket"
description: "记录下自己学习 WebSocket 相关的知识点，以及做一个基于 WebSocket 的简单的 BBS 系统的过程等"
date: 2018-01-24
tags: [Java, WebSocket, HTML5]
categories: [Tech]
---

>刷技术站点的时候，偶然间看到 WebSocket 的简介，觉得是个不错的技术，便想花费一点时间来学习一下。在此把学习的资料和过程简单记录一下，以便日后自己回顾，如果能给其他人一点帮助或者启发也未尝不是一件好事。在学习过程中，自己尝试使用 WebSocket 做点小东西结合理论知识加深对 WebSocket 的理解。

### Contents
* [What is WebSocket][what-is-websocket-contents-link]
* [Why do we need WebSocket][why-do-we-need-websocket-contents-link]
* [More details][more-details-contents-link]
* [Reference][reference-contents-link]

---

[what-is-websocket-contents-link]: #1-what-is-websocket
[why-do-we-need-websocket-contents-link]: #2-why-do-we-need-websocket
[more-details-contents-link]: #3-more-details
[reference-contents-link]: #4-reference


### 1. What is WebSocket

> WebSocket 是一种在单个 TCP 连接上进行全双工通讯的协议。WebSocket 通信协议于2011年被 IETF 定为标准 RFC 6455，并由 RFC7936 补充规范。WebSocket API 也被 W3C 定为标准。

以上是来自 wikipedia 的词条信息。词条中关于 WebSocket 是什么的描述很精简，其中有这么几个关键词：`TCP 连接`，`全双工`，`通讯的协议`，`标准`。

首先从理解上边这几个关键词开始。
* `通讯的协议`

    首先是理解这个`协议`，WebSocket 是一个用于`客户端`与`服务器`之间通信的协议(这里的客户端一般为浏览器)。这样一来，就有个问题，WebSocket 和如今常用的浏览器与服务器间通信的 `HTTP/HTTPS` 协议有什么关系？之间又有什么异同呢？

    那先来了解下 WebSocket 与 HTTP/HTTPS 之间有什么关系。下图是 WebSocket 的工作原理图(在此仅用于展示 WebSocket 与 HTTP 之间的关系，具体工作原理稍后再做较详细说明)，整张图表示一个完整的 WebSocket 通信过程。其中包括浏览器与服务器之间的握手过程，握手成功之后的双向多次通信，以及不再需要通信而由一方关闭连接或者由于异常情况导致的连接中断。其中红色箭头线为 HTTP 协议的请求与响应。那么可以通过这张图了解到，*`WebSocket 协议中使用了 HTTP 协议来处理握手过程`*。

    ![WebSocket 与 HTTP 间的关系][websocket-img-1]

    那么 WebSocket 和 HTTP 之间有什么异同呢？其实两个协议之间，必然有很多协议定义及实现上的细节性差别。这里想要通过比较来了解的异同，是作为一个使用者，从使用角度来看待它们之间的异同。先从相同点来看。
    * 两者都是用于`浏览器`与`服务器`之间通信的协议(这里只是从常用角度来看，当然服务器端也可以发起 HTTP 请求等)
    * 都是建立在 TCP 协议之上(这是从 OSI 模型角度来看，HTTP 和 WebSocket 都应用层的协议，所以需要底层的网络协议来支撑)
    
    再看看两者的不同点。
    * HTTP 为单向协议，也就是只能由客户端发请求数据给服务端，然后服务端针对这次请求数据返回响应数据。而不能由服务端发起请求数据/响应数据到客户端。而 WebSocket 最大的特点就是客户端与服务端通过握手建立连接之后，双发都可以互发数据。
    * 协议标识符的区别，HTTP 的协议标识为 `http://` (如果加密则为 `https://`)，而 WebSocket 的协议标识为 `ws://` (如果加密则为 `wss://`)。下图或许可以更为直观的表示它们之间的区别

    ![WebSocket 与 HTTP 间协议标识区别][websocket-img-2]

    它们之间还有其他异同点，在此也就不一一列举了。

* `标准`

    WebSocket 协议是 HTML5 提出的，而仅仅有协议并不能真正工作，需要这个流程链路上的各个角色都支持才可以。而各个角色又有各种厂商，之间如果没有一个标准来统一，也是无法形成一个统一的 API 供使用者使用。WebSocket 中有客户端(浏览器)以及服务端。根据 wikipedia 词条中的描述，主流浏览器已经根据最新规范([RFC 6455](https://tools.ietf.org/html/rfc6455))全部支持 WebSocket 了，而服务端，就拿 Java 系来说吧。各个服务器厂商都已支持[JEE JSR356 标准规范](https://www.jcp.org/en/jsr/detail?id=356)了。

    简而言之，就是各个角色都遵循标准，给使用者一个统一的 API 来方便地使用 WebSocket。

* `TCP 连接`

    该关键字所描述的就是 WebSocket 是建立在 TCP 协议的基础上的。

* `全双工`
    
    怎么理解全双工呢？还是先用 wikipedia 上词条的解释来做个铺垫。

    >全双工（full-duplex）的系统允许二台设备间同时进行双向数据传输。一般的电话、手机就是全双工的系统，因为在讲话时同时也可以听到对方的声音。
    全双工的系统可以用一般的双向车道形容。两个方向的车辆因使用不同的车道，因此不会互相影响。

    和其他的通讯协议做个比较或许更便于理解，那和 HTTP 协议做个简单的比较说明。
    HTTP 属于半双工（half-duplex）的协议，半双工的系统允许二台设备之间的双向数据传输，但不能同时进行。因此同一时间只允许一设备发送数据，若另一设备要发送数据，需等原来发送数据的设备发送完成后再处理。结合实际的 HTTP 协议应用来说，该半双工系统的两个设备就是`浏览器`和`HTTP 服务器`，一个 HTTP 通讯过程大致为`浏览器`发起一个请求，`HTTP 服务器`接收到这个请求之后，经过一定程度的业务处理，再返回一个返回信息给`浏览器`（忽略了 HTTP 握手过程）。

    下图分别是 `HTTP` 的半双工式通信，和 `WebSocket` 的全双工式通信过程。可以看到，半双工是一个请求一个响应对应的，一来一去的交互过程。而第二张图全双工的流程是在建立了链接之后，双方都在收发数据。

    ![半双工图示][websocket-img-3]
    ![全双工图示][websocket-img-4]

    第一张讲述 `HTTP` 半双工通信过程的图其实是基于 `HTTP/1.0` 版本及之前版本的。那就再顺便了解下 `HTTP/1.0` 和 `HTTP/1.1` 版本之间较大的一个差异。这个差异就是`持久连接`。

    下图左边是 `HTTP/1.0` 的多次连接形式，每一个请求和响应过程，都需要浏览器发起到服务器的连接，接收到响应之后关闭连接，下次请求再发起连接。建立 `HTTP` 连接是个相对耗费资源的操作，这种频繁地建立断开连接，其实消耗了很多资源。为了优化这个步骤，`HTTP/1.1` 时，加入了`持久连接`的概念。简单说就是第一次请求时，浏览器和服务器之间建立连接，第一次请求响应完成之后，该连接并不关闭，而是保持连接状态，之后的请求/响应都是在这个连接之上做的。直到不再需要通信或者其他原因才会断开连接。这样就节省了频繁建立和销毁连接所消耗的资源。其实这两种方式还有更为常用的别名，就是`短连接`和`长连接`。

    ![使用多个连接和使用持久链接的对比][websocket-img-5]

对于 WebSocket 是什么就先了解这么多。简而言之，WebSocket 就是一个 HTML5 的一个新的协议，和 HTTP 一样基于 TCP 来传输数据，借用 HTTP 来完成一部分工作，实现了浏览器和服务器的全双工通信，以及 HTTP 所不具有的服务器主动推送数据给浏览器的能力。

---

[websocket-img-1]: {{ site.url }}/assets/posts_img/websocket/WebSockets-Diagram.png
[websocket-img-2]: {{ site.url }}/assets/posts_img/websocket/HTTP_WebSocket_compare.jpg
[websocket-img-3]: {{ site.url }}/assets/posts_img/websocket/half_duplex.jpg
[websocket-img-4]: {{ site.url }}/assets/posts_img/websocket/full_duplex.jpg
[websocket-img-5]: {{ site.url }}/assets/posts_img/websocket/HTTP_persistent_connection.svg


### 2. Why do we need WebSocket

每一个技术的出现，基本上都是为了解决一些问题。那 WebSocket 的出现，是为了解决什么问题，而我们为什么需要这样一种技术呢？

那来聊聊 WebSocket 的出现是为了解决什么问题。做过 Web 开发的人或许碰到过这样的需求。页面上某个地方需要展示一些`实时`的数据。比如说 Web 版本的社交应用，或许是群聊，或许是两人之间的聊天。那聊天的内容就需要实时展示在页面上。基于 HTTP 协议的 Web 应用由于没有服务端主动发送数据到客户端的能力，这种`实时`的实现就变得较为困难了。所以这种 B/S 架构的应用就需要一个能够实现服务端主动推送数据到客户端的技术。

那我们为什么需要 WebSocket 呢？其实上边所说的基于 HTTP 协议来做浏览器`实时`展示服务端数据不是不能实现，只是实现上不够直接。在 WebSocket 之前，通常有以下几种实现方式。

* 轮询
* 长轮询
* Comet
* Flash Socket等等

这里仅稍微聊聊轮询和长轮询，其他的实现技术可以参考[服务器推送技术(请科学上网)][push-technology-link]。

既然 HTTP 不能由服务端直接发数据给客户端，那么就由客户端不停地问服务端有没有数据就可以了吧。这就是轮询的思路，就是由客户端每隔一段时间来发起 HTTP 请求，询问服务端有没有数据更新，如果有，就从服务端的响应中获取更新的数据，如果没有就下次再问。这样就可以做到`准实时`，如果实时性要求高，就将两次询问的时间间隔缩短即可。那么这将带来一些问题，就是每次询问都要创建 HTTP 连接，每次询问完再关闭连接，这样频繁地创建和关闭连接，将是很大的资源消耗。还有就是客户端会频繁地访问服务端，将给服务端带来很大的压力。

长轮询其实就是基于 `HTTP/1.1` 版本的`持久连接`，第一次建立 HTTP 连接之后不关闭它，在这个连接上多次询问，就节省了频繁创建和销毁连接的资源。但浏览器还是会频繁访问服务端，对于服务端的压力还是没有减轻。于是 WebSocket 就出现了。

WebSocket 使服务端可以主动发送数据给客户端，那么就不再需要浏览器频繁询问服务端是否有更新数据，而是服务端有更新数据时直接推送给浏览器。这样由于轮询或者长轮询带来的服务端压力自然减轻了。

---

[push-technology-link]: https://zh.wikipedia.org/wiki/%E6%8E%A8%E9%80%81%E6%8A%80%E6%9C%AF


### 3. More details

WebSocket 是什么，以及它能干什么也聊得差不多了，那从技术角度来稍微了解下其细节。我们还是从这张图开始。

![WebSocket 通讯流程图][websockets-diagram-img]

假如客户端创建了一个新的 WebSocket 对象，想要连接一个 WebSocket URL 是 `ws://localhost:8080/Chat` 的服务端。客户端会自动解析并识别这是个 WebSocket 请求。并使用 HTTP/1.1 协议的101状态码来进行握手。为了创建 WebSocket 连接，需要通过浏览器发出请求，之后服务器进行回应，这个过程通常称为“握手”(handshaking)。

以下截图是这个 WebSocket 握手过程的请求和响应报文。那我们从这个握手报文开始讲起。

![WebSocket Request General 图][websocket_general-img]

![WebSocket Request 图][websocket_request-img]

![WebSocket Response 图][websocket_response-img]

先来看看握手的请求报文，了解下其中几个重要的字段。
```
HTTP/1.1 101 Switching Protocols
Server: Apache-Coyote/1.1
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Accept: SpL4e0WlHRqRAsrLnSKVSf+vsEk=
Sec-WebSocket-Extensions: permessage-deflate;client_max_window_bits=15
Date: Fri, 26 Jan 2018 06:38:17 GMT
```
从报文第一行可以看到这个请求使用了 HTTP/1.1 协议。状态码是101，意思是转换通讯协议。Connection 必须设置 Upgrade，表示客户端希望连接升级。Upgrade 字段必须设置 Websocket，表示希望升级到 WebSocket 协议。其他几个比如 Sec-WebSocket-Accept 等字段可以参考[wikipedia 关于 WebSocket 词条的介绍(请科学上网)][reference-link-1]。

再来看看响应报文(为节省篇幅，省略了 User-Agent 等字段)，也是了解下几个重要字段。
```
GET ws://localhost:8080/Chat HTTP/1.1
Host: localhost:8080
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Version: 13
Accept-Encoding: gzip, deflate, br
Sec-WebSocket-Key: FdIprE1OQVLwTMFlMl0UKw==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```
同样，响应也是基于 HTTP/1.1 协议。Connection: Upgrade 表示服务端也同意协议升级，而 Upgrade: websocket 则表示协议升级为 WebSocket。

浏览器收到服务端返回的响应，就代表握手成功，后续就可以通过 TCP 来通讯了。对应本节的第一张图，就是中间蓝色的线。客户端和服务端都可以主动发送数据给对方。等双发不再需要通信时，可以由任何一端断开 TCP 连接。

---

[websockets-diagram-img]: {{ site.url }}/assets/posts_img/websocket/WebSockets-Diagram.png
[websocket_general-img]: {{ site.url }}/assets/posts_img/websocket/WebSocket_general.png
[websocket_request-img]: {{ site.url }}/assets/posts_img/websocket/WebSocket_request.png
[websocket_response-img]: {{ site.url }}/assets/posts_img/websocket/WebSocket_response.png


### 4. Reference

* [wikipedia 关于 WebSocket 词条的介绍(请科学上网)][reference-link-1]
* [@Ovear关于知乎“WebSocket 是什么原理？为什么可以实现持久连接？”的回答][reference-link-2]
* [java WebSocket 开发入门 WebSocket][reference-link-3]
* [WebSocket 介绍][reference-link-4]
* [WebSocket 和 Socket 的区别][reference-link-5]

---

[reference-link-1]: https://zh.wikipedia.org/wiki/WebSocket
[reference-link-2]: https://www.zhihu.com/question/20215561/answer/40316953
[reference-link-3]: https://www.jianshu.com/p/d79bf8174196
[reference-link-4]: https://hpbn.co/websocket/
[reference-link-5]: https://www.jianshu.com/p/59b5594ffbb0


