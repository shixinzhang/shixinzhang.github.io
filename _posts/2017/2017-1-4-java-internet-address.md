---
layout: post
title: Java 网络编程之 Internt 地址
categories: Java 网络编程
description: 了解 Internt 地址相关的 API 
keywords: Java 网络编程
---

可以连接到 Internet 的设备称为 “节点”，计算机节点称为 “主机”。

每个节点或者主机至少有一个数来唯一标识，这叫做 Internet 地址或者 IP 地址。

目前主要使用的是 IPv4 地址，四字节长；新版的 IPv6 地址有 16 字节长。

IPv4 地址四个字节的范围均为 0 ~ 255，

``127.0.0.1``，这叫点分四段格式

IPv6 格式的格式是被冒号分割的 8 个区块，每个区块是 4 个十六进制的数字，比如：

```
2400:cb00:2048:0001:0000:0000:6ca2:c665
```

> 两个冒号可以表示多个 0 区块，但是每个地址中双冒号至多能出现一次。

IP 很难记住，于是有了域名系统。域名服务器 DNS （Domain Name Server）负责关联域名和 IP。

**负载均衡**：

> 有时一个域名会映射到 多个 IP 地址，这时需要由 DNS 服务器负责随机选择一台机器来响应请求。
> 这个特性在业务流量非常大的 Web 网站经常使用，将负载分摊到多个系统上。


# InetAddress

``InetAddress`` 可以实现 DNS 的功能，在它的基础上做了缓存。

**不是所有 ``InetAddress`` 都有主机名**


域名和 IP 的对应关系除了本地，可能还会缓存到 Internet 中的其他 DNS 服务器上，因此修改后生效可能需要一段时间。

从主机名创建一个新的 InetAddress 是一个不安全的做法，因为这需要进行一个 DNS 查找；

相反，以字符串形式的 IP 地址创建 InetAddress，不会进行 DNS 查找。

![](http://oqg4nua5z.bkt.clouddn.com/inetaddress/inetaddress-0.png)

所有组播地址都以 FF 开头。

``InetAddress`` 有两个 ``isReachable()`` 方法，用于测试一个节点当前主机是否可达（即是否能建立一个网络连接）。

> 连接阻塞的原因：防火墙、代理服务器、行为失常的路由器，或者远程主机未启动

```
public boolean isReachable(int timeout) throws IOException {
    return isReachable(null, 0 , timeout);
}
public boolean isReachable(NetworkInterface netif, int ttl,
                           int timeout) throws IOException {
    if (ttl < 0)
        throw new IllegalArgumentException("ttl can't be negative");
    if (timeout < 0)
        throw new IllegalArgumentException("timeout can't be negative");

    return impl.isReachable(this, timeout, netif, ttl);
}
```

这些方法尝试使用  ``ICMP echo`` 请求查看指定地址是否可达。


``InetAddress`` 也实现了 Object 的方法，只要两个 ``InetAddress`` 对象的 IP 地址相同，即认为他们是相等的，不要求有相同的主机名。

``InetAddress.toString()`` 的实现：

```
public String toString() {
    String hostName = holder().getHostName();
    return ((hostName != null) ? hostName : "")
        + "/" + getHostAddress();
}
```

生成的字符串是这样的形式：主机名/点分四段地址

## Inet4Address 和 Inet6Address

```
public finalclass Inet4Address extends InetAddress {...}
public finalclass Inet6Address extends InetAddress {...}
```

分别代表 IPv4 和 IPv6，不过在应用程序中，一般不需要了解究竟是 v4 还是 v6，即使需要，只需要检查 ``getAddress()`` 所返回的字节数组的大小，是 4 还是 16 。

# NetworkInterface 类

```
/**
 * This class represents a Network Interface made up of a name,
 * and a list of IP addresses assigned to this interface.
 * It is used to identify the local interface on which a multicast group
 * is joined.
 *
 * Interfaces are normally known by names such as "le0".
 */
```

``NetworkInterface`` 表示一个**本地 IP 地址**。

- 可以是一个物理接口，比如以太网卡；
- 也可以是一个虚拟接口，与机器的其他 IP 地址绑定到同一个物理硬件。

``NetworkInterface`` 提供了一些方法可以列出所有本地地址，用它们创建 ``InetAddress`` 对象，然后拿它们创建 socket 等。

获取 ``NetworkInterface`` 的三种方法：

1. 根据名称
2. 根据 IP 地址
3. 枚举所有

```
private static void testNetworkInterface() {
    try {
        //UNIX 系统上以太网接口名为 eth0
        //Windows 名字类似 CE31 和 ELX100，具体名称取决于网络接口的厂商名和硬件模型名
        NetworkInterface ni = NetworkInterface.getByName("eth0");
        if (ni == null){
            System.out.println("No such interface: eth0");
        }

        //返回与指定 IP 绑定的网络接口
        NetworkInterface networkInterface1 = NetworkInterface.getByInetAddress(
                InetAddress.getByName("127.0.0.1"));
        if (networkInterface1 != null){
            System.out.println("getByInetAddress：" + networkInterface1);
        }

        //本地回送地址类似： lo

        //列出本地主机上的所有网络接口，联网和不联网的显示结果不同
        Enumeration<NetworkInterface> networkInterfaces = NetworkInterface.getNetworkInterfaces();
        while (networkInterfaces.hasMoreElements()){
            NetworkInterface networkInterface = networkInterfaces.nextElement();
            System.out.println(networkInterface);
        }
    } catch (SocketException e) {
        e.printStackTrace();
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
}

```

运行结果：

```
No such interface: eth0
getByInetAddress：name:lo0 (lo0)
name:awdl0 (awdl0)
name:en0 (en0)
name:lo0 (lo0)

Process finished with exit code 0
```

有了  ``NetworkInterface`` 以后就可以查询其 IP 地址和名字：

```
//列出本地主机上的所有网络接口，联网和不联网的显示结果不同
Enumeration<NetworkInterface> networkInterfaces = NetworkInterface.getNetworkInterfaces();
while (networkInterfaces.hasMoreElements()){
    NetworkInterface networkInterface = networkInterfaces.nextElement();
    System.out.println(networkInterface);

    //输出这个 NetworkInterface 下的所有 IP 信息
    Enumeration<InetAddress> inetAddresses = networkInterface.getInetAddresses();
    while (inetAddresses.hasMoreElements()){
        InetAddress inetAddress = inetAddresses.nextElement();
        System.out.println(inetAddress);
    }
}
```

输出结果：

```
name:awdl0 (awdl0)
/fe80:0:0:0:5403:69ff:fea3:cc29%awdl0        //v6
name:en0 (en0)
/fe80:0:0:0:c6b3:1ff:feb7:d959%en0
/192.168.1.6
name:lo0 (lo0)
/fe80:0:0:0:0:0:0:1%lo0
/0:0:0:0:0:0:0:1
/127.0.0.1

Process finished with exit code 0
```


