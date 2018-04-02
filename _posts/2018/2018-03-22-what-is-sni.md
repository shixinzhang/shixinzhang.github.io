---
layout: post
title: 什么是 SNI
categories: 网络编程
description: SSL 的一个拓展组件
keywords: SSL SNI
---

# SNI (Server Name Indication)

> 本文翻译自：https://www.thesslstore.com/blog/what-is-sni/

## SNI: 在同一个 IP 地址上运行多个 SSL 证书

SNI 即 Server Name Indication (服务名称证明)。

SNI 是一个关键但又比较神秘的 SSL 组件，它允许同一个 IP 地址上存在多个网站。不使用 SNI 的话，为了安装一个 SSL 证书，每个 IP 地址只能有一个域名。但 SNI 解决了这个问题。

上面这段话啥意思呢，我们慢慢道来。

## 为什么指定 Host 名称的方法无法再适用于 SSL 协议

大概在 2007 年以前，人们通过在 Header 里指明 Host 名称来解决这种同一 IP 上有多个网站的问题。

当客户端请求指定的网站时，在 HTTP Header 上指明要去的 Host 名称，然后服务端就会匹配 header 指定的网站，然后将请求发到那里去。

然而当 HTTPS 开始使用后，这一招不管用了。

因为 SSL 在客户端和服务端建立一个加密连接前，需要进行一次 SSL 握手。包含在 HTTP Header 里的域名只有在握手成功后才能被拿到，因此服务端无法知道该去连接同一 IP 上的哪个服务器。

## SNI 是什么

SNI（服务名称证明)是一个 SSL/TLS 协议的拓展，它允许在一个 IP 上有多个 SSL 证书。

SNI 是通过在 SSL 握手时插入 HTTP Header 实现这个功能的。因为服务端可以在握手时读到想要连接的 Host 名称，就可以连接到正确的网站。

![](https://blogs.akamai.com/assets_c/2017/03/handshake-diagram-thumb-500xauto-5789.png)

在 SNI 创造之前，每个你想加密的网站都必须有一个唯一的 IP 地址。你可能意识不到的是，这多么的浪费，甚至对 IPv4 IP 地址的消耗产生了非常严重的影响。

我们都知道 IPv4 定义的 IP 地址是有限的，它赋予使用 IP 协议进行通讯的电脑网络中的每一个设备一个地址。

一个 IPv4 的地址格式如下：

![](https://i0.wp.com/www.thesslstore.com/blog/wp-content/uploads/2017/11/ipv4-100629207-orig.png?w=774&ssl=1)

世界上大概有 40 亿个 IPv4 地址，迟早有一天这 40 亿个地址将都用完。但有了 SNI 以后，这个过程会慢很多，每个 IP 可以有多个服务器地址了。

最终所有的互联网将都使用 IPv 地址，这个版本下大概有 340 * 11^60 个地址，这个数字很大，大概可以够我们用很久了。

## SNI 的未来会怎样？

SNI 最让人担心是它的可扩展性。起初，一些人认为 Web 浏览器和服务器得好久以后才会使用 SNI。

事实证明，这种担忧在很大程度上是毫无根据的。今天，根据 [Akamai 统计](https://blogs.akamai.com/2017/03/reaching-toward-universal-tls-sni.html)，几乎 98% 的客户要求启用 HTTPS 的站点支持 SNI。

## 总结一下

这篇文章讨论了这些点：

1. 什么是 SNI：Server Name Indication （服务端名称证明）允许在同一个 IP 地址上存在多个 SSL 证书
2. SNI 在 SSL 握手中插入 HTTP Header 从而让服务端觉得连接哪个网站
3. 到 20171117 日为止，百分之 98 的用户要求 HTTPS 支持 SNI

# Thanks

- https://www.thesslstore.com/blog/what-is-sni/
- http://blog.csdn.net/makenothing/article/details/53292335

