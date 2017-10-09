---
layout: post
title: Java 网络编程之 HTTP 与 Cookie
categories: Java 网络编程
description: Java 网络编程之 HTTP 与 Cookie
keywords: Java 网络编程 HTTP Cookie
---

HTTP 是万维网数据通信的基础。

超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是一种用于分布式、协作式和超媒体信息系统的**应用层协议**。

HTTP 的发展是由 蒂姆·伯纳斯-李 于 1989 年在欧洲核子研究组织（CERN）所发起。设计 HTTP 最初的目的是为了提供一种发布和接收 HTML 页面的方法。

通过 HTTP 或者 HTTPS 协议请求的资源由统一资源标识符（Uniform Resource Identifiers，URI）来标识。

HTTP 是应用层协议，底层基于 TCP/IP，但是它也可以使用其他传输层协议实现。HTTP 假定其下层协议提供可靠的传输。因此，任何能够提供这种保证的协议都可以被其使用。目前在 TCP/IP 协议族中使用 TCP 作为其传输层。

![](http://oqg4nua5z.bkt.clouddn.com/http/http-evolution.jpg)

# HTTP 协议基础

HTTP 使用 TCP/IP 来传输数据，因此对于每个客户端到服务器的请求、响应，都有 4 个步骤：

1. 客户端在 80 端口（不另外指定的话）打开与服务器的一个 TCP 连接
2. 客户端向服务器发送请求
3. 服务器向客户端发送想要
4. 服务器关闭连接

**这是 HTTP 1.0 的过程。在 HTTP 1.1 及以后，一个 TCP 连接可以连续发送多个请求和响应，也就是上面的第二三步可以重复多次。**


**请求头的的字段如下**：

![](http://oqg4nua5z.bkt.clouddn.com/http/http_request_header_0.jpg)

描述 body 的请求头：

![](http://oqg4nua5z.bkt.clouddn.com/http/http_request_header_body.jpg)

MIME 类型包括两部分：类型和子类型

- 类型概况地描述要展示的是何种数据：图片、文本还是影片
- 子类具体的描述数据的特定类型：GIF 图像、JPEG 图像还是其他什么

已定义的 8 个顶级类型：

1. text/* 表示人可读的文字
2. image/* 表示图片
3. model/* 表示 3D 模型，如 VRML 文件
4. audio/* 表示声音
5. video/* 表示移动的图片，可能包括声音
6. applicaition/* 表示二进制数据
7. message/* 表示协议特定的信封，如 email 消息和 HTTP 响应
8. multipart/* 表示多个文档和资源的容器

每个类型分别有很多不同的子类型。

**响应码**

- 1XX **需要进一步提供信息**
 - 100 客户端在发送大量数据前询问服务器是否接受请求，得到这个响应后可以发送了
 - 101 服务端接受客户端要求改变应用协议的请求（如将 HTTP 改为 WebSockets）
- 2XX **请求成功**
 - 200，没问题
- 3XX **重定位及重定向，去别处找找看**
 - 300 服务器为请求的文档提供多种选择
 - 301 资源永久移动到新的 URL
 - 302 资源暂时移动到新的 URL，不久将来还会再次改变
 - 304 Not Modified，没更新，客户端应当从缓存中加载这个数据
- 4XX **客户端出错了**
 - 400 客户端语法不正确
 - 401 没有认证信息，token 或者用户名没有
 - 403 服务器理解请求，但拒绝处理
 - 404 服务器找不到所请求的资源，可能移走或者 URL 拼写错误
 - 405 请求方法不支持，换一种
 - 406 所请求的资源无法以客户端希望的格式提供（客户端期望的格式由 Header 的 ``Accept`` 字段表示）
 - 407 代理服务器要求客户端先进行身份认证
- 5XX **服务器错误**
 - 500 服务器内部错误
 - 501 服务器不具有处理该请求的能力
 - 502 表示该代理在试图完成请求时，从远端服务器得到一个无效的响应
 - 504 代理服务器等待远端服务器响应超时
 - 505 服务器不支持客户端正在使用的 HTTP 版本
 - 507 服务器没有足够的空间来存放所提供的数据
 - 511 客户端需要身份认证

## 每个版本的特点

### HTTP 1.0

第一个在请求、响应中指定 HTTP 的版本，至今仍被广泛使用，缺点是没有长连接。

### HTTP 1.1

**采用了持久链接，同时支持以管道的方式同时发送多个请求，以提高传输速度。**

HTTP 1.1 和 1.0 的区别：

- 缓存处理
- 带宽优化及网络连接的使用
- 加密，安全性提高

### HTTP 2.0

HTTP 2.0 主要基于 Google 发明的 SPDY 协议，对 HTTP 传输做了以下优化：

- 首部压缩
- 管道传输请求和响应
- 异步联接，多路复用

不过这些优化都是在传输层实现，对使用者来说不容易感知到。

# Keep-Alive


![](http://oqg4nua5z.bkt.clouddn.com/http/http-keep-alive.png)

一个 Web 会话中，**打开和关闭链接所花费的时间远远大于实际传输数据的时间**。

> 尤其是用 SSL 或者 TLS 加密的 HTTPS 连接，这个问题更为严重。

HTTP 1.0 会为每个请求打开一个新的连接；而在 HTTP 1.1 及以后，服务器不必发送响应后就关闭连接，可以保持连接打开，在同一个 socket 上等待客户端的新请求；可以在一个 TCP 连接上连续发送多个请求和响应。

客户端可以在 HTTP Header 中加一个字段：

```
Connection: Keep-Alive
```

这表示它希望重用一个 Socket。

如何修改系统属性来控制 Java 使用 HTTP Keep-Alive:

![](http://oqg4nua5z.bkt.clouddn.com/http/http-keep-alive-property.jpg)

# HTTP 方法

 HTTP 的请求与响应是无状态的。

![](http://oqg4nua5z.bkt.clouddn.com/http/http-request.jpg)

每个请求主要包括四个部分：

1. 请求行（包含 HTTP 方法、请求的资源路径和 HTTP 版本，例如：``GET /images/logo.gif HTTP/1.1``）
2. 请求头 （Key-Value 形式，例如：``Accept-Language: en``）
3. 空行
4. 请求体 （表示要提交的资源）

> 请求行和标题必须以<CR><LF>作为结尾。空行内必须只有<CR><LF>而无其他空格。


**HTTP 方法主要有这几种**：

1. GET：表示获取一个资源，不会修改服务器内容
2. PUT：有幂等性（idempotent），即重复多次也不会有副作用，后一次可以把前一次的覆盖了。因此一般用于更新，而不是新增
3. POST：没有幂等性，每次请求都会有影响，一般用于添加资源到服务器
4. DELETE：也是幂等的，用于删除
5. HEAD：简化版的 GET，只返回资源的首部，而不是返回具体数据，用于获取关于资源的描述信息（元信息）
6. OPTIONS：询问服务器该资源可以支持的所有 HTTP 请求方法
7. TRACE：回显客户端请求来进行调试

GET 用于非提交的动作，比如浏览网页；POST 则用于提交某个东西的动作。

POST 如今有点被滥用了，实际上不完成提交的所有安全操作都应当使用 GET 而不是 POST。

> 如今所有主流浏览器都能很好地应对至少 2000 个字符的 URL。

除非在请求资源时有大于 2000 个字符的数据，才需要使用 POST。

一般来讲，在 POST, PUT 请求向服务器发送数据时，最好在 HTTP 首部包括两个字段来描述请求体：

1. ``Content-length`` 指定请求体中有多少字节
2. ``Content-type`` 指定请求体的数据类型

# Cookie

很多网站使用一些小文本串在连接之间存储持久的客户端状态，这些小文本串称为 cookie。

一般流程是服务端返回给客户端的 Header 中包含 ``Set-Cookie: key=value``，告知客户端某个信息需要持久保存；然后客户端下次请求时在 Header 中发回这个 cookie。

比如这样，服务端返回的 Header:

```
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: cart=ZXCVBNM
```

客户端再次请求是，在头部增加 cookie:

```
GET /index.html HTTP/1.1
Host: www.shixinzhang.top
Cookie: cart=ZXCVBNM
...
```

**服务器可以设置多个 cookie**，即给客户端中返回的 Header 中可以有多个 ``Set-Cookie:XX=XX`` 信息。


## Cookie 作用域限制

Cookie 可以有多个属性来控制它们的作用域，包括：过期日期、路径、域、端口、版本等。


### 域名限制

默认情况下，Cookie 来自哪个服务器就只能应用于哪个服务器。

如果一个 cookie 由 www.shixinzhang.top 返回，这个 cookie 就只在这个域名下有用。

不过服务器可以在 ``Set-Cookie`` 里添加 ``Domain`` 属性表示这个 cookie 应用到哪个子域，比如下面这个信息表示为 整个 ``.blog.shixinzhang.top`` 设置一个用户 cookie：

```
Set-Cookie: user=ZXCVBNM; Domain=.shixinzhang.top
```

这样客户端不仅可以把这个 cookie 发给 shixinzhang.top，还可以发给 blog.shixinzhang.top, categories.shixinzhang.top 等等，也就是这个域名下的所有子域名。

不过只能是当前网站域名下的，不可以把 cookie 设置给其他网站，这种“第三方 cookie” 一般会被阻塞。

### 路径限制

Cookie 的作用域还受路径限制。

默认作用域是最初的 URL 和所有子目录，当然我们可以指定这个 cookie 作用的某个路径，比如下面这个信息表示 这个 cookie 只应用于服务器的 ``/about`` 子目录下，而不能应用于网站的其他部分：

```
Set-Cookie: user=ZXCVBNM; Path=/about
```

只在同服务器上子树 ``/about`` 中的资源时，这个 cookie 有用。

Cookie 可以同时包括域和路径，比如这样：

```
Set-Cookie: user=ZXCVBNM; Path=/about; Domain=.shixinzhang.top
```

服务器返回时不同属性的顺序不重要，只要用分号隔开即可；不过当客户端把 cookie 发回服务器时，路径必须在前面。

### 过期日期

可以给 cookie 中添加 ``expires`` 属性表示过期日期，格式为 “Wdy, DD-Mon-YYYY HH:MM:SS GMT”。
``Wndy`` 表示星期几，``Mon`` 表示月份，它俩都用 3 字母缩写形式。
其他部分都是数字。

在 ``java.text.SimpleDateFormat`` 中可以使用这个格式 ``E, dd-MMM-yyy k:m:s 'GMT'``。

比如下面的 cookie 表示在 2015 年 12 月 21 日下午 3:23 过期：

```
Set-Cookie: user=ZXCVBNM; expires=Wed, 21-Dec-2015 15:23:00 GMT
```

此外我们也可以使用 ``Max-Age`` 属性表示在几秒后过期，而不是特定时刻。比如下面的，表示在第一次设置之后的一小时过期：

```
Set-Cookie: user=ZXCVBNM; Max-Age=3600
```

客户端应该在过期之后删除这个 cookie。


为了避免 cookie 窃取攻击（如 XSRF），cookie 可以设置 HttpOnly 属性，表示让客户端值通过 HTTP/HTTPS 返回 cookie，而不能通过 JavaScript 代码，比如这样：

```
Set-Cookie: user=ZXCVBNM; Max-Age=3600; secure; httponly
```

## Java API

这里介绍几个 Java 中关于 Cookie 的 API，它们和 Java 网络请求的关系是这样的：

```
*                  use
* CookieHandler <------- HttpURLConnection
*       ^
*       | impl
*       |         use
* CookieManager -------> CookiePolicy
*             |   use
*             |--------> HttpCookie
*             |              ^
*             |              | use
*             |   use        |
*             |--------> CookieStore
*                            ^
*                            | impl
*                            |
*                  Internal in-memory implementation
```

### HttpCookie

``HttpCookie`` 存储了 cookie 的键值对和其他属性：

```
public final class HttpCookie implements Cloneable {
    // ---------------- Fields --------------

    // The value of the cookie itself.
    private final String name;  // NAME= ... "$Name" style is reserved
    private String value;       // value of NAME

    // Attributes encoded in the header's cookie fields.
    private String comment;     // Comment=VALUE ... describes cookie's use
    private String commentURL;  // CommentURL="http URL" ... describes cookie's use
    private boolean toDiscard;  // Discard ... discard cookie unconditionally
    private String domain;      // Domain=VALUE ... domain that sees cookie
    private long maxAge = MAX_AGE_UNSPECIFIED;  // Max-Age=VALUE ... cookies auto-expire
    private String path;        // Path=VALUE ... URLs that see the cookie
    private String portlist;    // Port[="portlist"] ... the port cookie may be returned to
    private boolean secure;     // Secure ... e.g. use SSL
    private boolean httpOnly;   // HttpOnly ... i.e. not accessible to scripts
    private int version = 1;    // Version=1 ... RFC 2965 style

    // The original header this cookie was consructed from, if it was
    // constructed by parsing a header, otherwise null.
    private final String header;

    // Hold the creation time (in seconds) of the http cookie for later
    // expiration calculation
    private final long whenCreated;

    // Since the positive and zero max-age have their meanings,
    // this value serves as a hint as 'not specify max-age'
    private final static long MAX_AGE_UNSPECIFIED = -1;

    // date formats used by Netscape's cookie draft
    // as well as formats seen on various sites
    private final static String[] COOKIE_DATE_FORMATS = {
        "EEE',' dd-MMM-yyyy HH:mm:ss 'GMT'",
        "EEE',' dd MMM yyyy HH:mm:ss 'GMT'",
        "EEE MMM dd yyyy HH:mm:ss 'GMT'Z",
        "EEE',' dd-MMM-yy HH:mm:ss 'GMT'",
        "EEE',' dd MMM yy HH:mm:ss 'GMT'",
        "EEE MMM dd yy HH:mm:ss 'GMT'Z"
    };

    // constant strings represent set-cookie header token
    private final static String SET_COOKIE = "set-cookie:";
    private final static String SET_COOKIE2 = "set-cookie2:";
    //...
}
```

### CookieManager

Java 5 中有一个管理 cookie 的抽象类 ``CookieHandler`` ，它定义了存储和获取 cookie 的方法：

```
public abstract class CookieHandler {
    //系统范围的单例
    private static CookieHandler cookieHandler;
    public synchronized static CookieHandler getDefault() {
        SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            sm.checkPermission(SecurityConstants.GET_COOKIEHANDLER_PERMISSION);
        }
        return cookieHandler;
    }
    public synchronized static void setDefault(CookieHandler cHandler) {
        SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            sm.checkPermission(SecurityConstants.SET_COOKIEHANDLER_PERMISSION);
        }
        cookieHandler = cHandler;
    }
    public abstract Map<String, List<String>>
        get(URI uri, Map<String, List<String>> requestHeaders)
        throws IOException;
    public abstract void
        put(URI uri, Map<String, List<String>> responseHeaders)
        throws IOException;
}
```

Java 6 中 ``java.net.CookieManager`` 继承了这个抽象类，实现了方法，源码这里暂不展开。

默认情况下 cookie 并不打开，我们可以这样启用 cookie:

```
CookieManager manager = new CookieManager();
CookieHandler.setDefault(manager);
```

这样你用 URL 类连接的服务器，从服务器响应中接受 cookie，在发送请求时添加到头部，就这么实现了！简单吧。

### CookiePolicy

如果你希望对接受哪种 cookie，就需要为 ``CookieManager`` 指定一个 ``CookiePolicy``。

``CookiePolicy`` 是一个接口，内部有 3 个静态实现类：

```
public interface CookiePolicy {
    /**
     * 接收所有 cookie
     */
    public static final CookiePolicy ACCEPT_ALL = new CookiePolicy(){
        public boolean shouldAccept(URI uri, HttpCookie cookie) {
            return true;
        }
    };

    /**
     * 不接受任何 cookie
     */
    public static final CookiePolicy ACCEPT_NONE = new CookiePolicy(){
        public boolean shouldAccept(URI uri, HttpCookie cookie) {
            return false;
        }
    };

    /**
     * 只接受第一方 cookie，即只接受与你对话的服务器的 cookie，不接受其他第三方服务器发送的
     */
    public static final CookiePolicy ACCEPT_ORIGINAL_SERVER  = new CookiePolicy(){
        public boolean shouldAccept(URI uri, HttpCookie cookie) {
            return HttpCookie.domainMatches(cookie.getDomain(), uri.getHost());
        }
    };


    //子类需要实现的方法，用于表示是否接受这个 cookie
    public boolean shouldAccept(URI uri, HttpCookie cookie);
}
```

使用时很简单，比如这样：

```
CookieManager manager = new CookieManager();
manager.setCookiePolicy(CookiePolicy.ACCEPT_ORIGINAL_SERVER);
CookieHandler.setDefault(manager);
```

如果你想自定义策略，只需要实现这个接口即可。

### CookieStore

``CookieStore`` 也是一个接口，它定义 cookie 在本地存储的添加和删除。

```
public interface CookieStore {
    public void add(URI uri, HttpCookie cookie);

    public List<HttpCookie> get(URI uri);

    public List<HttpCookie> getCookies();

    public List<URI> getURIs();

    public boolean remove(URI uri, HttpCookie cookie);

    public boolean removeAll();
}
```

它的实现类目前只有 ``InMemoryCookieStore``，它通过几个 List 和 Map 保存 Cookie 信息：

```
class InMemoryCookieStore implements CookieStore {
    // the in-memory representation of cookies
    private List<HttpCookie> cookieJar = null;

    // the cookies are indexed by its domain and associated uri (if present)
    // CAUTION: when a cookie removed from main data structure (i.e. cookieJar),
    //          it won't be cleared in domainIndex & uriIndex. Double-check the
    //          presence of cookie when retrieve one form index store.
    private Map<String, List<HttpCookie>> domainIndex = null;
    private Map<URI, List<HttpCookie>> uriIndex = null;

    // use ReentrantLock instead of syncronized for scalability
    private ReentrantLock lock = null;


    /**
     * The default ctor
     */
    public InMemoryCookieStore() {
        cookieJar = new ArrayList<HttpCookie>();
        domainIndex = new HashMap<String, List<HttpCookie>>();
        uriIndex = new HashMap<URI, List<HttpCookie>>();

        lock = new ReentrantLock(false);
    }

    /**
     * Add one cookie into cookie store.
     */
    public void add(URI uri, HttpCookie cookie) {
        // pre-condition : argument can't be null
        if (cookie == null) {
            throw new NullPointerException("cookie is null");
        }


        lock.lock();
        try {
            // remove the ole cookie if there has had one
            cookieJar.remove(cookie);

            // add new cookie if it has a non-zero max-age
            if (cookie.getMaxAge() != 0) {
                cookieJar.add(cookie);
                // and add it to domain index
                if (cookie.getDomain() != null) {
                    addIndex(domainIndex, cookie.getDomain(), cookie);
                }
                if (uri != null) {
                    // add it to uri index, too
                    addIndex(uriIndex, getEffectiveURI(uri), cookie);
                }
            }
        } finally {
            lock.unlock();
        }
    }
    //...
}
```

当我们要在本地存放和获取 Cookie 时，就需要使用它了，一般我们通过 ``CookieManager.getCookieStore()`` 方法获取这个接口的实现：

```
public CookieStore getCookieStore() {
    return cookieJar;
}
```

# Thanks

https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE
http://liuwangshu.cn/application/network/1-http.html
