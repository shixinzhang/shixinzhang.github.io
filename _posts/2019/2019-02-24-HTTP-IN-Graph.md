---
layout: post
title: 《图解 HTTP》读书笔记
categories: Java 网络编程
description: 零碎的笔记还是整理下吧
keywords: 网络 基础
---


这本书读了有段时间了，最近又翻出来读了读，顺便把笔记整理一下吧。

![](https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=8dbaa65f067b02080cc938e75ae295ee/9d82d158ccbf6c8163b6a212b03eb13532fa4043.jpg)

网络协议经典书：



- 《HTTP 权威指南》

- 《TCP/IP 详解，卷1》



前端工程师可能对炫酷效果的页面更感兴趣，但是网络协议这部分基础内容，才能决定你能否在专业技术道路上走的更远、更坚实。对基础及核心部分的深入学习，是成为一名专业技术人员的前提，**以不变应万变才是立足之本。**



>HTTP 通信过程中，代理、网关和隧道的作用？

>何为无状态？

>301 和 302 的区别？



# 协议



协议即规则。



计算机与网络设备要相互通信，双方就必须有“共同语言”，即有相同的通信规则。比如，如何探测到通信目标，由哪一边先发起通信，怎样结束通信等规则。



协议中定义了很多内容，从电缆的规格到 IP 地址的选定方法、寻找异地用户的方法、双方建立通信的顺序，以及 Web 页面显示需要处理的步骤等。



把与互联网相关联的协议统称为 ``TCP/IP``。



# TCP/IP 协议簇



分层管理，4 层：应用层、传输层、网络层、数据链路层。



1. 应用层（定义了向用户提供应用服务时通信的格式）

 - HTTP

 - FTP（File Transfer Protocol ，文件传输协议）

 - DNS（Domain Name System，域名系统）

2. 传输层（提供处于网络连接中的两台计算机之间的数据传输）

 - TCP（Transmission Control Protocol，传输控制协议）

 - UDP（User Data Protocol，用户数据包协议）

3. 网络层（处理在网络上流动的数据包，数据包是网络传输的最小数据单位）

 - IP 协议（Internet Protocol 互联网协议），**几乎所有使用网络的系统都会用到 IP 协议**

 - 该层规定了通过怎样的路径到达对方计算机，并把数据传送给对方

4. 数据链路层（处理连接网络的硬件部分）

 - 包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card 网卡），及光纤等物理部分



>应传网数，“应传网速”



**用 HTTP 请求举例来说明 4 层的合作过程**：



1. 首先，客户端在应用层（HTTP 协议）发出一个想看某个 Web 页面的 HTTP 请求

2. 接着，为了传输方便，传输层（TCP 协议）把从应用层收到的数据（HTTP 请求报文）进行分割，并给各个报文打上标记序号及端口号，然后转发给网络层

3. 在网络层（IP 协议），增加作为通信目的地的 MAC 地址后转发给链路层，至此通信请求已准备完全

4. 数据在链路层中传输后到达服务端，从下往上挨层取出内容，最后到达服务端应用层，才算真正接收到客户端发送的 HTTP 请求





**为什么数据传输要分包？**因为网络传输是不稳定的，如果一次性传输大量数据，失败了就得全部重新传输；而分包后，有部分包传输失败，只需要重新传输失败的包即可。



>数据分包模式下的问题：数据的拆分和组装



发送端在层与层之间传输数据时，每经过一层会被加上该层所属的头部信息；反之，在接收端，每经过一层会把对应的头部去掉。



**书 P39 图**



## IP 地址和 MAC 地址



IP 协议的作用是保证数据包传送到对方。



传输到对方需要满足许多条件，其中两个重要的条件是 IP 地址和 MAC 地址要正确。



> MAC 地址（Media Access Control）



- IP 地址指明了节点被分配到的地址，**可变换**

- MAC 地址是指网卡所属的固定地址，**基本不会变**

- IP 地址可以和 MAC 地址进行配对



**IP 地址间的通信依赖 MAC 地址。**



通信双方如果不在同一局域网，通常需要经过多台计算机和网络设备中转才能连接到对方。在进行中转时，会利用下一站中转设备的 MAC 地址来搜索下一个中转目标。这时，需要采用 ``ARP`` 协议（Address Resolution Protocol）。



>ARP 用以解析地址的协议，根据通信方的 IP 地址可以反查出其 MAC 地址，反之亦然。



**书 P 45，52 图**



>为什么 IP 地址不设计成固定的？有 MAC 地址和 ARP 就可以找到了，没必要设计成固定的。



无论哪台计算机、网络设备，它们无法全面掌握互联网中的细节，它们只知道大概的信息，比如下站送往哪里。其实人生也一样，我们无法知道整个人生会怎样，唯一能知道的，就是当下该怎么走。不用纠结怎样做才能快速进步，做好手上的每件事，自然就会有新的好事降临。



## 确保可靠性的 TCP 协议



TCP 协议可以确认数据最终是否到达对方。



**三次握手**：用 TCP 协议把数据发出后，不会就此结束，TCP 会向对方确认是否成功送达。



握手过程中使用了 TCP 的标志位 ``SYN`` 和 ``ACK``：



1. C --SYN-->  S

2. C <--SYN/ACK-- S

3. C --ACK--> S



>若在握手过程中某个阶段莫名中断，TCP 协议会以相同的顺序发送相同的数据包。



**TCP 其他确保通信可靠性的手段：**



- 四次挥手

- 数据包序号机制

- 确认机制

- 滑动窗口

- 拥塞控制算法等 



## URI



URI 就是由某个协议方案表示的资源的定位标识符，“协议方案”+“资源”+“定位标识”。协议方案：http, ftp, mailto, telnet, file 等。


# HTTP 协议



网页浏览器内输入地址，显示出网页的过程？



- 浏览器根据指定 URL 的 Web  服务器那里获取文件资源信息，然后把返回的结果展示出来。

- 这里发出请求、和解析响应的过程中，使用 HTTP （HyperText Transfer Protocol）协议作为规范



>``Apache``：现在已然成为 Web 服务器标准之一。



HTTP 于 1990 年问世，那时还没有作为正式的标准。正式被作为标准是在 1996 年的 5 月。1997年 1 月公布的 HTTP/1.1 是目前主流的 HTTP 协议版本。



当年 HTTP 协议的出现主要是**为了解决文本传输的难题**，协议本身非常简单。



**兼容问题的起源**：1995 年起，微软和网景之间爆发的浏览器大战，两家公司各自对  HTML  做了拓展，导致在写 HTML 页面时，需要考虑兼容两家公司的浏览器。


## HTTP 是无状态的协议



HTTP 协议自身不对请求和响应之间的通信状态进行保存。但是为了实现保存状态的功能，于是引入了 ``Cookie``。



``Cookie`` 会根据服务端的响应报文里的 ``Set-Cookie`` 头部字段，通知客户端保存。当有 Cookie 后，客户端会自动在请求报文里添加 Cookie 的值。



>加入我 2017 年写的 Cookie 链接。



**书 P88 方法 Method 图**



## 持久连接



在 HTTP/1.1 中，所有的连接默认都是**持久连接**。但在 HTTP/1.0 并未实现标准化。



持久连接使得请求以管线化（pipelining）发送成为可能。



>管线化是指：同时并行发送多个请求，而不需要一个接着一个的等待响应了。


**书P 93 图**



##  内容编码



常用的内容编码方式：



1. gzip

2. compress（UNIX 系统的标准压缩）

3. deflate（zlib）

4. identity（不进行编码）



## 增量传输（获取部分内容范围的请求）



从之前下载中断处恢复下载。



在请求里指定下载的数据范围，这种请求叫做“范围请求”。



请求从一开始到 3000 字节和 5000~7000 字节的多重范围数据：



```

Range: bytes=-3000, 5000-7000

```



针对范围请求，响应会返回状态码为 **206 Partial Content** 的响应报文。另外，针对多重范围请求，响应会在首部字段 ``Content-Type`` 里标明 ``multipart/byteranges``。



## 内容协商



访问支持不同语言的Web 页面时，显示浏览器的默认语言对应的页面，这样的机制称为“内容协商 Content Negotiation”。



内容协商会以响应资源的语言、字符集、编码方式等作为判断的基准。例如：



- ``Accept``

- ``Accept-Charset``

- ``Accept-Encoding``

- ``Accept-Language``

- ``Content-Language``



内容协商技术有以下三种类型：



1. 服务器驱动协商（服务器根据请求的首部字段返回对应的页面）

2. 客户端驱动协商（客户端自己根据 OS 类型或者 浏览器类型自行切换）

3. 透明协商



## 不太常用的状态码



- 204 No Content：服务器接收的请求已成功处理，但返回的响应报文里没有主体

 - 用于只需要从客户端往服务器发送信息。服务器不需要返回数据

- 206 Partial Content：表示客户端进行了范围请求，而服务器成功执行了这部分请求，响应报文里包含 Content-Range 指定范围的实体内容

- 301 Moved Permanently：永久性重定向

 - 资源地址彻底修改，以后应使用 ``Location`` 首部字段提示的 URI

- 302 Found：临时性重定向，资源暂时需要使用新地址访问，后面还会恢复

- 303 See Other：和 302 功能相同，但它明确表示，客户端应当采用 ``GET`` 方法获取资源。

- 307 Temporary Redirect：和 302 含义相同

- 304 Not Modified：服务器资源未改变，可直接使用客户端未过期的缓存

 - 304 虽然被分在 3xx 里，但和重定向没有关系

- 403 Forbidden：未获得文件系统的访问授权

- 503 Service Unavailable：服务器暂时处于超负载或者正在进行停机维护



## 网关、代理、隧道



>网关：转发其他服务器通信数据的服务器。

>代理：每次通过代理服务器转发请求或响应时，会追加写入 Via 首部信息



使用代理服务器的理由：



1. 利用缓存技术减少网络带宽的流量

2. 组织内部针对特定网站的访问控制

3. 以获取访问日志为主要目的等



利用网关可以由 HTTP 请求转化为其他协议通信；利用网关能提高通信的安全性，因为可以在客户端与网关之间的通信线路上加密，以确保连接的安全。



隧道的目的是确保通信的安全。使用 SSL 建立安全通信线路之后，就可以安全的进行 HTTP 通信了。



## HTTP 的缺点 *



1. 使用明文通信，可能被窃听

2. 不验证通信方身份，可能遭遇伪装

3. 无法证明报文完整性，可能被篡改



>中间人攻击



# HTTPS



与 SSL 组合使用的 HTTP 被称为 HTTPS 或 HTTP over SSL。



## HTTP + 加密 + 认证 + 完整性保护 = HTTPS



>HTTPS 并非是应用层的新协议，只是把 HTTP 通信部分用 SSL 和 TLS 协议代替而已。



HTTP 直接和 TCP 通信，当使用 SSL 时，就变成先和 SSL 通信，再由 SSL 和 TCP 通信了。



![](https://ws1.sinaimg.cn/large/b1aad299gy1g0hryglzjmj20j208ower.jpg)



- SSL（Secure Sockets Layer 安全套接层）

 - 提供功能：加密、认证、摘要

- TLS（Transport Layer Secure 传输层安全）



SSL 不仅提供加密处理，还使用了证书用于确认身份。



> SSL 是独立于 HTTP 的协议，所以不光是 HTTP 协议，其他应用层协议例如 SMTP 和 Telnet 都可以配合 SSL 使用。



## 加密机制



HTTPS 采用混合加密机制，即使用对称加密加密数据，使用非对称加密加密密钥。



因为非对称加密（公开密钥加密）比对称加密（共享密钥加密）处理速度更慢，所以只能用它加密密钥，加密数据的话影响传输性能。



![](https://ws1.sinaimg.cn/large/b1aad299gy1g0hs3qmim7j20l80j2gni.jpg)



## 证明公钥正确性的证书



上面的加密有个问题，如何证明收到的公钥是正确的呢？在公钥传输的过程中，有可能被替换了。



解决办法就是：**使用由数字证书认证机构（CA Certificate Authority）颁发的公钥证书。**



>数字证书认证机构  是客户端和服务端都可信任的第三方。



### 数字证书认证机构的业务流程



![](https://ws1.sinaimg.cn/large/b1aad299gy1g0hsecuf9tj20o50il414.jpg)



1. 服务器运营人员向``数字证书认证机构``申请公钥（花钱）

2. 证书机构确认身份后，对公钥做数字签名，然后把公钥和证书绑定在一起（应该就是建立某种数学关系）

3. 服务器将由 ``数字证书认证机构``颁发的**公钥证书（也可叫数字证书或者直接叫证书）** 发送给客户端，以使用公钥加密通信内容

4. 客户端拿到证书后，可使用服认证机构的公钥对证书上的数字签名进行验证

 - 验证通过，则证明服务器的公钥是有效的



第四点里，认证机构的公钥如何安全转交给客户端是个问题，一般浏览器会内置常用机构的公钥。



### 客户端证书



用以证明客户端的身份，一般有两种：



1. 机构颁发证书

 - 比较贵，每张证书都需要花钱，一般用于网上银行业务

2. 自签名证书：由自认证机构颁发的证书



## HTTPS 的安全通信机制 *



![](https://ws1.sinaimg.cn/large/b1aad299gy1g0hsub7hhvj20o80tvgnx.jpg)



**HTTPS 的通信步骤：**



1. SSL 握手协商

2. 公钥交换



### SSL 握手协商



1. 客户端发送报文，开始 SSL 通信

 - 报文中包含客户端支持的 SSL 版本、加密组件（Cipher Suite）列表（即使用的加密算法和秘钥长度等）

2. 服务端可进行 SSL 通信时，会发送响应报文

 - 响应里也包含 SSL 版本和加密组件（客户端的子集）

3. 服务端发送 Certificate 报文，包含公钥证书

4. 服务端发送握手结束报文通知客户端，最初的 **SSL Handshake 握手协商** 过程就结束了



### 公钥交换



接着前面的第四步，SSL 第一次握手结束后，客户端拿到公钥证书，然后验证其有效性，有效后：



5.客户端发送 ``Client Key Exchange`` 报文，**报文里包含一个非常重要的随机数，这个随机数用步骤 3 里的公钥加密了，它就是后面的数据通信使用的秘钥**

6.接着客户端又发生 ``Change Cipher Spec`` 报文，提示服务器以后通信使用前一条发送的随机数加密

7.客户端发送 ``Finished ``报文，这个报文里会包含连接至今全部报文的整体校验值

 - 这次握手协商能否成功，以服务器能否正确解密该报文作为判定标准

8.服务端同样发送 ``Change Cipher Spec`` 

9. 服务器接着也发送  ``Finished ``报文



服务器和客户端的 Finished 报文交换完毕后，SSL 连接就算建立完成。后面的应用层协议通信，会受到 SSL 的保护。



![](https://img-blog.csdn.net/20161226114828559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXpoMTk5Mg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



**在使用 SSL 后，应用层发送数据时会附加一直叫做 MAC （Message Authentication Code）的报文摘要，这个摘要可以用来确定报文是否遭到篡改，从而保护报文的完整性。**



## SSL 和 TLS



SSL 最初由浏览器开发商网景通信公司率先倡导的，SSL3.0 之前的版本是它们开发的。后来主导权转移到 IETF（Internet Engineering Task Force ，Internet 工程任务组）。



IETF 以 SSL3.0 为基准，后来制定了 TLS1.0, TLS1.1 和 TLS1.2。



>小结：TLS 是以 SSL 为原型开发的协议，它和 SSL 会被统称为 SSL。



HTTPS 比 HTTP 要慢 2 到 100 倍，是因为通信过程略复杂，除去 TCP 建立连接、HTTP 请求/响应以外，还多了一层 SSL（需要握手协商、交换密钥），此外报文都需要进行加密解密，所以整体上时间会长一些。



为何所有的 Web 网站不一直使用 HTTPS？



>因为与纯文本通信想比，加密通信会消耗更多的 CPU 及内存资源。如果每次通信都加密，服务器资源会占用更多，能够处理的请求数也会随之减少。



所以只有在包含个人信息等敏感数据时，才使用 HTTPS 加密通信。



# 认证



- BASIC 认证

- DIGEST 认证

- SSL 客户端认证

- 基于表单认证



# 基于 HTTP 的协议

- SPDY
- WebSocket
- WebDAV

# Web  安全



服务端应如何保存用户密码等登录信息没有标准化，一种安全的保存方法是：

