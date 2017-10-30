---
layout: post
title: Java 网络编程之 Proxy 类浅析
categories: Java 网络编程
description: Java 网络编程之 Proxy 类浅析
keywords: Java 网络编程 Proxy
---

有时候我们会使用代理，比如翻#和谐#墙。


大概流程是这样的：

- 代理服务器接收到本地客户端到远程服务器的请求
- 代理服务器向远程服务器发出请求
- 然后将结果转发给本地客户端

![](https://ws1.sinaimg.cn/large/b1aad299gy1fk9gya8dvgj20id09c0uz.jpg)

这样做有时候是为了安全，如防止远程主机了解本地客户端的信息；有时候是为了过滤请求，限制可以访问的网站。


# Proxy 类

Java 提供的 ``Proxy`` 类很简单：

```
public class Proxy {

    //代理类型
    public enum Type {
        /**
         * Represents a direct connection, or the absence of a proxy.
         */
        DIRECT,
        /**
         * Represents proxy for high level protocols such as HTTP or FTP.
         */
        HTTP,
        /**
         * Represents a SOCKS (V4 or V5) proxy.
         */
        SOCKS
    };

    private Type type;
    private SocketAddress sa;

    public final static Proxy NO_PROXY = new Proxy();

    private Proxy() {
        type = type.DIRECT;
        sa = null;
    }

    public Proxy(Type type, SocketAddress sa) {
        if ((type == Type.DIRECT) || !(sa instanceof InetSocketAddress))
            throw new IllegalArgumentException("type " + type + " is not compatible with address " + sa);
        this.type = type;
        this.sa = sa;
    }

    public Type type() {
        return type;
    }

    public SocketAddress address() {
        return sa;
    }

    public String toString() {
        if (type() == Type.DIRECT)
            return "DIRECT";
        return type() + " @ " + address();
    }
    //...
}
```

可以看到，``Proxy`` 类最关键的两个信息就是：

- ``Type`` 代理类型
- ``SocketAddress`` 代理地址

``Type`` 是一个枚举，分别表示：直接连接（无代理）、HTTP 代理、Socet 代理

```
    public enum Type {
        /**
         * Represents a direct connection, or the absence of a proxy.
         */
        DIRECT,
        /**
         * Represents proxy for high level protocols such as HTTP or FTP.
         */
        HTTP,
        /**
         * Represents a SOCKS (V4 or V5) proxy.
         */
        SOCKS
    };
```

``SocketAddress`` 的实现是 ``InetSocketAddress``，可以看到，主要信息就是代理服务器的 host, addr 和 port:

```
public class InetSocketAddress extends SocketAddress{
    // Private implementation class pointed to by all public methods.
    private static class InetSocketAddressHolder {
        // The hostname of the Socket Address
        private String hostname;
        // The IP address of the Socket Address
        private InetAddress addr;
        // The port number of the Socket Address
        private int port;
        //...
        }
}
```

使用例子：

```
private static void testProxyCreated() {
    SocketAddress socketAddress = new InetSocketAddress("proxy.shixinzhang.top", 80);
    Proxy proxy = new Proxy(Proxy.Type.HTTP, socketAddress);
}
```

``Proxy`` 类的作用就是允许我们对不同的主机提供不同的代理服务器。

# ProxySelector 类

每个运行的虚拟机都有一个 ``java.net.ProxySelector`` 对象，用来确定不同连接的代理服务器。

它就相当于一个代理服务器的表，URI 去这里捞一遍，看有没有代理，内容也很简单：

```
public abstract class ProxySelector {
    /**
     * The system wide proxy selector that selects the proxy server to
     * use, if any, when connecting to a remote object referenced by
     * an URL.
     *
     * @see #setDefault(ProxySelector)
     */
    private static ProxySelector theProxySelector;

    static {
        try {
            Class c = Class.forName("sun.net.spi.DefaultProxySelector");
            if (c != null && ProxySelector.class.isAssignableFrom(c)) {
                theProxySelector = (ProxySelector) c.newInstance();
            }
        } catch (Exception e) {
            theProxySelector = null;
        }
    }

    /**
     * Gets the system-wide proxy selector.
     *
     * @throws  SecurityException
     *          If a security manager has been installed and it denies
     * {@link NetPermission}<tt>("getProxySelector")</tt>
     * @see #setDefault(ProxySelector)
     * @return the system-wide <code>ProxySelector</code>
     * @since 1.5
     */
    public static ProxySelector getDefault() {
        SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            sm.checkPermission(SecurityConstants.GET_PROXYSELECTOR_PERMISSION);
        }
        return theProxySelector;
    }

    /**
     * Sets (or unsets) the system-wide proxy selector.
     *
     * Note: non-standard protocol handlers may ignore this setting.
     *
     * @param ps The HTTP proxy selector, or
     *          <code>null</code> to unset the proxy selector.
     *
     * @throws  SecurityException
     *          If a security manager has been installed and it denies
     * {@link NetPermission}<tt>("setProxySelector")</tt>
     *
     * @see #getDefault()
     * @since 1.5
     */
    public static void setDefault(ProxySelector ps) {
        SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            sm.checkPermission(SecurityConstants.SET_PROXYSELECTOR_PERMISSION);
        }
        theProxySelector = ps;
    }


    public abstract List<Proxy> select(URI uri);

    public abstract void connectFailed(URI uri, SocketAddress sa, IOException ioe);
}
```

默认的 ProxySelector 只检查各种系统属性和 URL 的协议，我们可以创建自己的 ProxySelector 来代替默认的选择器，自己实现根据协议、主机、路径等选择不同的代理。

```
public class LocalProxySelector extends ProxySelector {
    @Override
    public List<Proxy> select(final URI uri) {
        return null;
    }

    @Override
    public void connectFailed(final URI uri, final SocketAddress sa, final IOException ioe) {

    }
}
```

创建一个 ProxySelector 需要实现上面的两个方法，第一个方法 ``select()`` 传入一个 URI 对象，不同的链接会有不同的协议及组成：

- 用 URL 类生成的链接，通常为 http://xxx 或者 ftp://ftp.xxxx
- 用 Socket 类生成的纯 TCP 链接，URI 形式为 socket://host:port

根据 URI 为它选择不同的代理，返回到 ``List<Proxy>`` 中。

第二个方法 ``connectFailed()`` 用于警告程序这个代理服务器没有建立连接。



## 改变默认 ProxySelector

每个虚拟机只有一个 ProxySelector，我们自定义后，可以调用 ``ProxySelector.setDefault(ourSelector)`` 修改。

从此虚拟机打开的所有链接都要向这个 ProxySelector 询问要使用的代理。

> 一般不建议在共享环境的代码中使用这个方法，比如不要在 ``servlet`` 中改变代理选择器。


