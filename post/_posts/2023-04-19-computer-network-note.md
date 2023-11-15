---
layout: post_note
title:  "计算机网络 | 学习笔记"
description: 
type: card-dated
date:   2020-10-29 20:01:21 -0400
image:  # for local images, place in /assets/img/posts/
caption:
last-updated: 2020-10-26 20:01:21 -0400
categories: post
tag: note
author: Marco LIU
card: card-2
toc: true
---

2023/04/18 小马

本学习笔记框架与内容基于2023字节前端训练营和网络资料。仅供本人学习、记录、温习用。

# 网络组成

**主机，路由器，交换机，...**

信息交换方式： 电路交换，**分组交换**

<p align="center"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3208767bacae4c7ca3ad9dea6da3c480~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

# OSI网络协议/分层

**OSI: Open Systems Interconncection 开放系统互联**

网络协议包括**标头（header）和载荷（payload）**。

### 网络分层

1.  物理层
2.  数据链路层 (**MAC**)
3.  网络层 (**IP**)
4.  传输层 (传输控制协议**TCP**和用户数据报协议**UDP**)
5.  会话层
6.  表示层
7.  应用层 (**HTTP** **DNS**)

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/090e448140fe4fee962c278771f1cbb7~tplv-k3u1fbpfcp-zoom-1.image" alt="" width="100%">

每一层协议，除了上一层的标头和载荷，还有本层的标头。

**强烈推荐阅读↓**

> 有关物理层、数据链路层、网络层（前三层）协议、网络通信原理；MAC地址与IP地址区别:
>
> <https://mp.weixin.qq.com/s/jiPMUk6zUdOY6eKxAjNDbQ>


<p align="center"><img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4f8abc38c6b34fbea43a87ae410beefa~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

# TCP协议(三次握手、四次挥手)

## TCP三次握手

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/906e35387bea4e68b97f54c72009ef61~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

1.  客户端发送一个带 SYN=1，Seq=X 的数据包到服务器端口
    *   浏览器发起，告诉服务器 **我要发送请求了**
2.  服务器发回一个带 SYN=1， ACK=X+1， Seq=Y 的响应包以示传达确认信息
    *   服务器发起，告诉浏览器 **我准备接受了，你赶紧发送吧**
3.  客户端再回传一个带 ACK=Y+1， Seq=Z 的数据包，代表“握手结束”
    *   浏览器发送，告诉服务器 **我马上就发了，准备接受吧**

**为什么需要三次握手，2次不行吗?**

*   如果只有 2 次的话，B 并不清楚 A 是否收到他发过去的信息。

## TCP四次挥手

<p align="center"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/52aad54a1d9f4f5999e2577540f33aad~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

1.  浏览器发起，告诉服务器，我**请求报文发送完了**，你准备关闭吧
2.  服务器发起，告诉浏览器，我**请求报文接收完了**，我准备关闭了，你也准备吧
    *   A 到 B 的连接已经释放，**不再接收 A 发的数据了**。但是因为 TCP 连接是双向的，所以**B 仍旧可以发送数据给 A**
    *   此时还有没发完的数据会继续发送
3.  服务器发起，告诉浏览器，我**响应报文发送完了，你准备关闭吧**
4.  浏览器发起，告诉服务器，我**响应报文接收完了，我准备关闭了，你也准备吧**
    *   此时 A 进入 TIME-WAIT 状态。该状态会持续 2MSL(最长报文段寿命，指报文段在网络中生存的时间，超时会被抛弃) 时间, 若该时间段内没有 B 的重发请求的话，就进入 CLOSED 状态
    *   当 B 收到确认应答后，也便进入 CLOSED 状态。

> **来源/推荐阅读**：
>
> <https://www.51cto.com/article/661212.html>
> <https://juejin.cn/post/6844903784229896199#heading-4>

# UDP和TCP协议对比

TCP、UDP都是是**传输层**协议：

## UDP (User Datagram Protocol) 用户数据报协议 ：

*   无连接;
*   尽最大努力的交付;
*   面向报文;
*   无拥塞控制;

## TCP (Transmission Control Protocol) 传输控制协议 ：

*   **面向连接**
    *   在真正的读写操作之前，客户端与服务器端之间必须建立一个连接，当读写操作完成后，双方不再需要这个连接时可以释放这个连接。连接的建立依靠“三次握手”，而释放则需要“四次握手”，所以每个连接的建立都是需要资源消耗和时间消耗的
*   每一个TCP连接只能是点对点的(一对一);
*   提供**可靠交付**服务;
*   面向字节流；

> **来源/推荐阅读**：<https://www.51cto.com/article/661212.html>

# HTTP 1.0\~3.0 协议对比

HTTP 是**建立在 TCP 上**的应用层协议，超文本传送协议。

HTTP 连接最显著的特点是：客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。

<p align="center"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/51e2b834cc1047889c16e2a7f014e931~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

**<p align=center>短连接 & 长连接 & HTTP流水线</p>**

## HTTP1.0

*   **短连接**: 浏览器**每次请求**都需要与服务器建立一个TCP连接（三次握手），服务器处理完成以后**立即断开**TCP连接，无法复用连接，浪费性能
*   队头堵塞 (Head of Line Blocking)： 由于规定下一个请求必须在前一个请求响应到达之前才能发送，若前一个请求响应一直不到达，那么下一个请求就不发送，后面的请求就阻塞了
*   无状态：借助cookie/session机制来做身份认证和状态记录
*   不支持断点续传：每次都会传送全部的页面和数据

## HTTP1.1

*   **长连接**：**可保持HTTP连接不断，一次连接中处理多个请求**。避免每次请求都要重复建立释放TCP连接。提高网络的利用率。
*   支持断点续传
*   可使用**管道传输**，多个请求可以同时发送，但是服务器还是按照顺序回应。如果前面的回应特别慢，后面就会有许多请求排队等着。**队头堵塞依旧存在**。
*   **无法多路复用**

## HTTP2.0

在应用层和传输层之间引入二进制分层**帧 (frame)**。帧是HTTP2.0通信的最小单位，每个帧包含头部，会标识出当前所属的流(stream_id)。

**优点**

*   **多路复用 (Multiplexing)/链接共享**：真正实现了**并行运输**，在一个TCP连接上实现任意数量的HTTP请求：
    *   所有HTTP2.0通信都在一个TCP链接上完成，这个链接可以承载任意流量的**双向**数据流(stream)。每个数据流以消息的形式发送，而消息由一或多个帧(frame)组成。这些帧可以**乱序**发送，然后再根据每个帧头部的流标识符(Stream_id)重新封装。
*   可调整优先级
    *   优先级高的数据流会被服务器优先处理和返回客户端
*   头部压缩
    *   使用encoder来减少需要传输的header大小，通讯双方各自cache一份header_files表，既避免重复header的传输，又减少了需要传输的大小
*   服务器推送 Server Push

<p align="center"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ae0b85230824730b0f8c14341c191b9~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="90%"></p>

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5e1baf6782514071a76a366f373fd32a~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="90%"></p>

**虽然但是...**

*   **TCP上依旧可能出现队头堵塞\~**

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb1f6eda34c9461fbee755e3382bade3~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

**3-RTT启动**

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3a8da23a7aa41ed9c9242904f04d2e3~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

## HTTP3.0

放弃TCP,使用**基于UDP的QUIC协议**。

*   加密后才能连接
*   **减少了原本的TCP以及TLS握手时间**，被合并到 QUIC 中的单个请求/响应周期中 (**1-RTT**)

<p align="center"><img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b93028cd587e492fba19ce1a068de4bf~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

*   **0-RTT**建连: QUIC相比HTTP2最大的性能优势（传输层0-RTT就能建立连接；加密层0-RTT就能建立加密连接）
    *   本地**缓存**当前会话的上下文，下次恢复会话的时候，只需要将之前的缓存传递给服务器，验证通过，就可以进行传输了。

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3f8312427b804b8abd2d29fad08cedd7~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

*   **No more 队头堵塞**：解决了http 2.0中前一个stream丢包导致后一个stream被阻塞的问题

*   **多路复用**： 一个连接上的多个stream之间没有依赖，即使丢包，只需要重发丢失的包即可，不需要重传整个连接。

> **来源/推荐阅读**：
>
> <https://www.51cto.com/article/661212.html>
> <https://www.jianshu.com/p/cd70b8e90d00>
> <https://zhuanlan.zhihu.com/p/266578819>

# HTTP和TCP对比

*   TCP 协议对应于传输层，而 HTTP 协议对应于应用层
*   **HTTP 协议是在 TCP 协议之上建立的**，HTTP 在发起请求时通过 TCP 协议建立起连接服务器的通道，请求结束后，立即断开 TCP 连接
*   HTTP 是**无状态**的短连接，而 TCP 是**有状态**的长连接
*   TCP是传输层协议，定义的是数据传输和连接方式的规范，HTTP是应用层协议，定义的是传输数据的内容的规范

> **来源/推荐阅读**：
>
> <https://www.51cto.com/article/661212.html>

# SSL\&TLS协议

*   **SSL**: Secure Sockets Layer 安全套接层
    *   为网络通信提供安全及数据完整性的一种安全协议
    *   HTTPS是应用层的安全协议，TCP是传输层的协议，但是它不安全，因为它是**明文传输**的；SSL的诞生就是给TCP加了一层保险，使**HTTPS和TCP之间使用加密传输**
*   **TLS**: Transport Layer Security 安全传输层协议

    *   用于在两个通信应用程序之间提供保密性和数据完整性
    *   **TLS是SSL的升级版**，作用是一样的。
    *   TLS协议则位于**应用层和传输层之间**
    *   TCP握手是TLS握手的前提

# DNS (Domain Name System 域名系统)

DNS: **Domain Name System** 域名系统, 是互联网上作为域名和IP地址相互映射的一个分布式数据库; 通过域名，最终得到该域名对应的IP地址的过程叫做**域名解析**（或主机名解析）。

## DNS域名解析过程

**浏览器如何通过域名去查询 URL 对应的 IP ？**

*   DNS域名解析分为递归查询和迭代查询两种方式，现一般为**迭代查询**。

<p align="center"><img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/870cb1b39c974ab3867876b61e4111fc~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

1.  打开浏览器，输入一个域名（如 [www.google.com](http://www.google.com) ），这时你的电脑会发出一个DNS请求到**本地DNS服务器** （一般都是你的网络接入服务器商提供，比如中国电信/中国移动）。
2.  本地DNS服务器会首先查询它的**缓存记录**，如果缓存中有此条记录，就可以直接返回结果。如果没有，本地DNS服务器还要向**DNS根服务器**进行查询。
3.  根DNS服务器没有记录具体的域名和IP地址的对应关系，而是告诉本地DNS服务器，你可以到**域服务器**上去继续查询，并给出域服务器的地址。
4.  本地DNS服务器继续向**域服务器**发出请求，在这个例子中，请求的对象是.**com域服务器**。
5.  **.com域服务器**收到请求之后，也不会直接返回域名和IP地址的对应关系，而是告诉本地DNS服务器，你的**域名的解析服务器**的地址。
6.  最后，本地DNS服务器向**域名的解析服务器**发出请求，这时就能收到一个域名和**IP地址**对应关系，本地DNS服务器不仅要把IP地址返回给用户电脑，还要把这个对应关系保存在**缓存**中，以备下次别的用户查询时，可以直接返回结果，加快网络访问。

# CDN (content delivery network)

**基于DNS重定向**

<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ef46bbd987246b5b21dea59bfc2bf33~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

<p align="center"><img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3d9f747ffa84b7db0806ceac4f757eb~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

# WebSocket

*   **从HTTP协议升级而来**
*   **有状态**的持久连接
*   服务端可以主动推送消息
*   用 WebSocket 发送消息延迟比 HTTP 低

# 网络安全

### 网络安全三要素

*   机密性: 攻击者无法获知通信内容
*   完整性: 攻击者对内容进行篡改时能被发现
*   身份验证: 攻击者无法伪装成通信双方的任意一方与另一方通信
*   完整性&身份验证相互关联

### 对称加密&非对称加密

*   对称加密: 加密、解密用同样的密钥
*   非对称加密: 加密、解密使用不同的密钥 (公钥和私钥)**公钥加密只能用私钥解密、私钥加密只能用公钥解密**

### 密码散列函数 (哈希函数)

*   输入: **任意长度**的内容
*   输出: **固定长度**的哈希值
*   性质: 找到两个不同的输入使之经过密码散列函数后有相同的哈希值在计算上是**不可能**的

### 网络安全三要素的实现？

*   **机密性**：网络是明文的；想要通过明文通信交换秘密信息，通信双方需要**先有秘密信息**
*   **完整性**：<p align="center"><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6779852d7a884ae89d53667a7581c77b~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>
*   **身份验证**：**数字签名**

<p align="center"><img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cef09bba4cce40d798f7cb3216d3ecd0~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

*   **保证了机密性、完整性和身份验证**

    <p align="center"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/71621c931eef4437ac5b950a7312897c~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="80%"></p>

### HTTPS

HTTPS：Hyper Text Transfer Protocol over Secure Socket Layer

*   把 HTTP 的明文换成**密文**，再**验证身份**，即 HTTPS
    *   HTTPS = HTTP + **TLS**

    *   TLS = 身份验证 + 加解密

    *   身份验证靠 **PKI**

*   **服务端**身份验证靠**PKI**，**客户端**身份验证靠**HTTP 协议**

*   SSL 凭证安装于伺服器（例如网站服务器）上，但是在浏览器上，使用者仍可看到网站是否受到SSL的保护。如果网站受到SSL凭证保护，使用者看到的网址会是以https\:// 开头，而不是http\:// （多出的一个s 代表**安全**）

> 来源：<https://www.sohu.com/a/251746171_216613>

# 推荐阅读

> *   从URL输入到页面展现到底发生什么？ <https://juejin.cn/post/6844903784229896199#heading-5>
> *   从HTTP到WEB缓存
> <https://juejin.cn/post/6844903791809003527#heading-0>
