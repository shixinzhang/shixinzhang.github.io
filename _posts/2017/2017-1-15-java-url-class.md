---
layout: post
title: Java 网络编程之 URL 类
categories: Java 网络编程
description: Java 网络编程之 URL 的使用及相关方法
keywords: Java 网络编程 URL
---

了解如何确定 IP 地址后，进一步学习如何确定资源的地址。

# 基础概念

URL 是最常见的 URI （统一资源标识符）。

``URL`` 类是 Java 程序在网络上定位和获取数据的最简单办法。

> Web 和 URL 都是在 UNIX 下发明的。

## URI

统一资源标识符（Uniform Resource Identifier, URI），采用特定的语法**标识一个资源的字符串**。

语法为：

```
模式：模式特定部分
```

模式特定部分的语法取决于所用模式，模式包括：

- data （链接中直接包含的 Base 64编码数据）
- file （本地磁盘上的文件）
- ftp （FTP 服务器）
- http （使用超文本传输协议的国际互联网服务器）
- mailto （电子邮件）
- magnet （可以通过对等网络下载的资源）
- telnet （与基于 Telnet 的服务的连接）
- urn （统一资源名）

> http 只是其中一种

```
[//<authority>]<path>[?<query>]
```

``authority`` 标识负责解析该 URI 剩余部分的授权机构（authority）。


模式部分由小写字母、数字和加号、点、连号符组成；
路径和查询分别有 ASCII 字母数字符号组成（即 A-Z、a-z、0-9）。

**所有非 ASCII 字符，应当用百分号（%）转义，% 后跟着该字符按 UTF-8 编码的十六进制码。**

> 汉字会被转义，比如汉字 “木”，Unicode 码点为 0x6728，在 UTF-8 中，会编码为 3 字节 E6,9C 和 A8，因此它在 URI 中的编码为``%E6%9C%A8 ``。

## URL

**``java.net.URI`` 类只标识资源；``java.net.URL`` 类既能标识资源，又能获取资源。**

语法：

```
protocol://userInfo@host:port/path?query#fragment
```

- protocel 可以是 file, ftp, http, https, magnet, telnet 或其他字符串
- userInfo 是服务器的登录信息，不是必有的
- host 既可以是主机名，也可以是 IP 地址
- port 不是必有的，HTTP 服务器的默认端口是 80

``userInfo@host:port`` 构成 ``authority``。

- path 指向服务器的一个特定目录
 - **一般来讲，路径是相对于当前服务器的文档根目录，而不是整个文件系统的根目录**
 - 所有的路径和文件名都是相对于这个目录

> 向公众开放的服务器不会将整个文件系统展示给客户端，而只是展示指定目录中的内容。

## 相对 URL

完整指定的 URL 称为绝对 URL；继承了父文档的部分信息，不完整的 URL 称为相对 URL（relative URL）。

假设在浏览 <https://shixinzhang.top/about/aboutme.html> 时点击这个超链接：

```
<a href="index.html">
```

浏览器会截断尾部的 ``aboutme.html``，拼出新的 <https://shixinzhang.top/about/index.html>，然后加载这个文档。

# URL 类


```
public final class URL implements java.io.Serializable {...}
```


``java.net.URL`` 类是 final 的，不允许派生子类，此外 URL 也是不可变的，构造一个 URL 对象后，其字段不可改变，可以保证线程安全性。

## 创建新的 URL

``URL`` 的构造方式有多种：

```
public URL(String url) throws MalformedURLException
public URL(String protocol, String hostname, String file) throws MalformedURLException    //指定文件，file 的路径需要以斜线开头
public URL(String protocol, String host, int port, String file) throws MalformedURLException    //指定端口
public URL(URL base, String relative) throws MalformedURLException        //相对 URL
```

> 所有虚拟机都支持的协议只有 ``http``, ``file``。

检测当前虚拟机支持的协议：

```
    private static void testProtocolSupport() {
        //超文本传输协议
        printProtocol("http://www.shixinzhang.top");
        //安全的 http
        printProtocol("https://shixinzhang.top");
        //文本传输协议
        printProtocol("ftp://shixinzhang.top/java/xxx");
        //邮件传输协议
        printProtocol("mailto:sxzhang2016@163.com");
        //telnet
        printProtocol("telnet://xidian.edu");
        //本地文件访问
        printProtocol("file://etc/password");
        //gopher
        printProtocol("gopher://gopherxxx");
        //JAR
        printProtocol("jar:http://sdada.jar");
        //jdbc 的定制协议
        printProtocol("jdbc:mysql://lunaxx");
        //NFS 网络文件系统
        printProtocol("nfs://xxx");
    }

    private static void printProtocol(@NonNull final String urlString) {
        try {
            URL url = new URL(urlString);
            System.out.println(url.getProtocol() + " is supported!");
        } catch (MalformedURLException e) {
//            e.printStackTrace();
            String protocol = urlString.substring(0, urlString.indexOf(":"));
            System.out.println(protocol + " is not supported!");
        }
    }
```

运行结果：

```
http is supported!
https is supported!
ftp is supported!
mailto is supported!
telnet is not supported!
file is supported!
gopher is not supported!
jar is not supported!
jdbc is not supported!
nfs is not supported!

Process finished with exit code 0
```

**Java 不会对构造的 URL 进行检查，比如主机名是否包含空格等，都不会检查的。**

> ClassLoader 不仅用于加载类，也能加载资源，如图片和音频文件。

静态方法 ``ClassLoader`.getSystemResource()` 返回一个 URL，通过它可以读取一个资源。

```
public static URL getSystemResource(String name) {
    ClassLoader system = getSystemClassLoader();
    if (system == null) {
        return getBootstrapResource(name);
    }
    return system.getResource(name);
}
public URL getResource(String name) {
    URL url;
    if (parent != null) {
        url = parent.getResource(name);
    } else {
        url = getBootstrapResource(name);
    }
    if (url == null) {
        url = findResource(name);
    }
    return url;
}
```

## 从 URL 中获取数据

几个获取 URL 所指向文档数据的方法：

```
public URLConnection openConnection() throws java.io.IOException {
    return handler.openConnection(this);
}
public URLConnection openConnection(Proxy proxy)
    throws java.io.IOException {
    if (proxy == null) {
        throw new IllegalArgumentException("proxy can not be null");
    }

    // Create a copy of Proxy as a security measure
    Proxy p = proxy == Proxy.NO_PROXY ? Proxy.NO_PROXY : sun.net.ApplicationProxy.create(proxy);
    SecurityManager sm = System.getSecurityManager();
    if (p.type() != Proxy.Type.DIRECT && sm != null) {
        InetSocketAddress epoint = (InetSocketAddress) p.address();
        if (epoint.isUnresolved())
            sm.checkConnect(epoint.getHostName(), epoint.getPort());
        else
            sm.checkConnect(epoint.getAddress().getHostAddress(),
                            epoint.getPort());
    }
    return handler.openConnection(this, p);
}
public final InputStream openStream() throws java.io.IOException {
    return openConnection().getInputStream();
}
inal Object getContent() throws java.io.IOException {
    return openConnection().getContent();
}
public final Object getContent(Class[] classes)
throws java.io.IOException {
    return openConnection().getContent(classes);
}
```

### openStream()

``openStream()`` 方法可以连接到 URL 所引用的资源，在客户端和服务器之间完成必要的握手，返回一个输入流，可以由此读数据。

测试一下：

```
private static void testUrlOpenStream() {
    try {
        URL url = new URL("http://www.baidu.com");
        try(InputStream in = url.openStream()){
            int c;
            while ((c = in.read()) != -1){
                System.out.write(c);
            }
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>使用百度前必读</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a>&nbsp;京ICP证030173号&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>

Process finished with exit code 0
```

一般来讲，Web 页面如果使用了与 ASCII 完全不同的字符集，HTML 会在首部包括一个 ``META`` 标记，指示所使用的字符集。

比如上面这个：

```
<meta http-equiv=content-type content=text/html;charset=utf-8>
```

但是检测这个字符集比较费劲，因为在实际文档中 HTTP 首部的编码信息可能有多种，分别用于指定不同的文件。

### openConnection()

**如果希望与服务器直接通信，应当使用这个方法。**

``openConnection()`` 会为指定 URL 打开一个 socket，并返回一个 ``URLConnection`` 对象。


``URLConnection`` 表示一个网络资源打开的连接。

通过 URLConnection 我们可以访问服务器发送的所有数据，除了文档意外，还可以访问图片等，还可以写入数据。

```
/**
 * 通过 URL 获得 socket 连接
 */
private static void testUrlOpenConnection() {
    try {
        URL url = new URL("http://www.baidu.com");
        URLConnection urlConnection = url.openConnection();
        InputStream inputStream = urlConnection.getInputStream();   //读数据
        inputStream = new BufferedInputStream(inputStream);

        Reader r = new InputStreamReader(inputStream);
        int c;

        while ((c = r.read()) != -1) {
            System.out.print((char) c);
        }

        OutputStream outputStream = urlConnection.getOutputStream(); //还可以写数据
    } catch (MalformedURLException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

输出结果：

```
<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>使用百度前必读</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a>&nbsp;京ICP证030173号&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>

Process finished with exit code 0
```

### getContent()

``getContent()`` 可以获取由 URL 引用的数据，如果 URL 指示的是文本，返回的通常是 ``InputStream`` 子类；如果指示的是图片，返回的是一个 ``URLImageSource``。

访问一个网页：

```
private static void testUrlGetContent() {
    try {
        URL url = new URL(BAIDU_URL);
        Object content = url.getContent();
        System.out.println("content type : " + content.getClass().getName());
    } catch (MalformedURLException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
content type : sun.net.www.protocol.http.HttpURLConnection$HttpInputStream

Process finished with exit code 0
```

如果访问一个图片，返回结果是：

```
content type : sun.awt.image.URLImageSource

Process finished with exit code 0
```

> 使用 ``getContent()`` 最大的问题是：很难预测将获得那种对象。


## 分解 URL

URL 的组成及对应获取方法：

- 模式（也叫协议）``getProtocol()``
- 授权机构，``getAuthority()``, 授权机构进一步划分为：
 - 用户信息 ``getUserInfo()``
 - 主机 ``getHost()``
 - 端口 ``getPort()``，没有显式指定端口，返回 -1
- 路径 ``getPath()``，不包含查询字符串，只有路径
- 片段标识符(即 # 符号后面的字符) ``getRef()`` 
- 查询字符串("key=value" 这样子的) ``getQuery()``

``getDefaultPort()`` 返回默认端口，http 默认端口是 80，ftp 是 21.

``getFile()`` 也返回文件路径，但和 ``getPath()`` 不同的是，它会包含查询字符串。从主机名后的第一个斜线开始，一直到片段标识符（#），都被认为是文件部分。


## 相等性和比较

URL 的 ``equals()`` 和 ``hashCode()`` 当且仅当两个 URL 指向相同的主机、端口和路径上的相同资源，才会认为是相等的。

```
    protected boolean equals(URL u1, URL u2) {
        return Objects.equals(u1.getRef(), u2.getRef()) &&
               Objects.equals(u1.getQuery(), u2.getQuery()) &&
               // sameFile compares the protocol, file, port & host components of
               // the URLs.
               sameFile(u1, u2);
    }
```

**实际上，``equals()`` 方法会尝试用 DNS 解析主机，以判断主机是否相同，因此这可能是一个阻塞的 I/O 操作，要尽量避免影响后面的操作！**

```
private static void testUrlEquals() {
    try {
        URL www = new URL("http://www.shixinzhang.top");
        URL url = new URL("http://shixinzhang.top");
        System.out.println(www + " equals to " + url + "? " + www.equals(url));
    } catch (MalformedURLException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
http://www.shixinzhang.top equals to http://shixinzhang.top? true

Process finished with exit code 0
```

**说明 ``equals()`` 是会根据主机比较的。 **



``URL`` 还有一个 ``sameFile()`` 方法，用于比较两个 URL 是否指向相同的资源。这里也会包括 DNS 查询。



# GET 请求如此简单

本来想单独开一篇的，奈何使用 URL 实现 GET 请求太简单，就凑合写到一起吧：

```
private static void testGetRequest() {
    try {
        URL url = new URL("http://gank.io/api/data/Android/10/1");
        try (InputStream in = new BufferedInputStream(url.openStream())) {
            Reader reader = new InputStreamReader(in);
            int c;
            while ((c = reader.read()) != -1) {
                System.out.write(c);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    } catch (MalformedURLException e) {
        e.printStackTrace();
    }
}

```

使用 URL 类实现 GET 请求就是如此简单，不信你看运行结果：

```
{
  "error": false, 
  "results": [
    {
      "_id": "59c3d43a421aa9727ddd19c0", 
      "createdAt": "2017-09-21T23:01:14.453Z", 
      "desc": "\u7075\u6d3b\u7684ShadowView\uff0c\u53ef\u66ff\u4ee3CardView\u4f7f\u7528", 
      "images": [
        "http://img.gank.io/5ca55569-7b78-4691-aa1a-a1165eb8a164"
      ], 
      "publishedAt": "2017-09-29T11:21:16.116Z", 
      "source": "web", 
      "type": "Android", 
      "url": "https://github.com/loopeer/shadow", 
      "used": true, 
      "who": null
    }, 
    {
      "_id": "59c3db8d421aa9727ddd19c1", 
      "createdAt": "2017-09-21T23:32:29.351Z", 
      "desc": "Android Transition\u5b66\u4e60\u7b14\u8bb0", 
      "images": [
        "http://img.gank.io/a553aa37-9a54-4bb6-bdc2-9bfc2737e065"
      ], 
      "publishedAt": "2017-09-29T11:21:16.116Z", 
      "source": "web", 
      "type": "Android", 
      "url": "http://rkhcy.github.io/2017/09/21/TransitionNote/", 
      "used": true, 
      "who": "HuYounger"
    }, 
    {
      "_id": "59cc8cac421aa972879d121a", 
      "createdAt": "2017-09-28T13:46:20.656Z", 
      "desc": "Android\u4e09\u79cd\u59ff\u52bf\u5e26\u4f60\u73a9\u8f6c360\u5ea6\u5168\u666f\u56fe\u529f\u80fd", 
      "publishedAt": "2017-09-29T11:21:16.116Z", 
      "source": "web", 
      "type": "Android", 
      "url": "https://github.com/CN-ZPH/Three360panorama", 
      "used": true, 
      "who": "\u5f20\u9e4f\u8f89"
    }, 
    {
      "_id": "59cc9df6421aa9727ddd19db", 
      "createdAt": "2017-09-28T15:00:06.42Z", 
      "desc": "\u6bd4\u5b98\u65b9\u66f4\u50cf\u5b98\u65b9\u7684NationalGeographic\u56fd\u5bb6\u5730\u7406-\u6bcf\u65e5\u7cbe\u9009\u5ba2\u6237\u7aef\uff0c\u91cd\u8981\u7684\u662f\u7528\u4e86\u6700\u65b0\u7684Android Architecture Components(Lifecycle) & Kotlin\uff0c\u7ed9\u5927\u5bb6\u63a2\u8def\u586b\u5751", 
      "publishedAt": "2017-09-29T11:21:16.116Z", 
      "source": "web", 
      "type": "Android", 
      "url": "https://www.github.com/wheat7/NationalGeographic", 
      "used": true, 
      "who": "\u9ea6\u7530\u54e5"
    }, 
    {
      "_id": "59b74f4a421aa911847a0390", 
      "createdAt": "2017-09-12T11:06:50.144Z", 
      "desc": "\u57fa\u4e8eTesseract-OCR\u5b9e\u73b0\u81ea\u52a8\u626b\u63cf\u8bc6\u522b\u624b\u673a\u53f7", 
      "images": [
        "http://img.gank.io/d2c557fe-7d3b-4330-9c51-9c427178f633"
      ], 
      "publishedAt": "2017-09-26T12:12:07.813Z", 
      "source": "web", 
      "type": "Android", 
      "url": "https://github.com/simplezhli/Tesseract-OCR-Scanner", 
      "used": true, 
      "who": "\u552f\u9e7f"
    }, 
    {
      "_id": "59b9da64421aa911847a039a", 
      "createdAt": "2017-09-14T09:24:52.83Z", 
      "desc": "\u8be6\u7ec6\u8bb2\u89e3Launcher\u6574\u4e2a\u5f00\u53d1\u6d41\u7a0b\u7684\u7cfb\u5217\u6559\u7a0b\u3002", 
      "publishedAt": "2017-09-26T12:12:07.813Z", 
      "source": "web", 
      "type": "Android", 
      "url": "http://www.codemx.cn/tags/Launcher/", 
      "used": true, 
      "who": "YUCHUAN"
    }, 
    {
      "_id": "59bbe41f421aa9118887ac26", 
      "createdAt": "2017-09-15T22:30:55.891Z", 
      "desc": "Android7.1 \u7684 HashMap\u5de5\u4f5c\u539f\u7406", 
      "publishedAt": "2017-09-26T12:12:07.813Z", 
      "source": "web", 
      "type": "Android", 
      "url": "http://www.jianshu.com/p/1f0b8175ce7c", 
      "used": true, 
      "who": "Niekon"
    }, 
    {
      "_id": "59c89a0d421aa9727fdb25dc", 
      "createdAt": "2017-09-25T13:54:21.702Z", 
      "desc": "Drakeet \u7684 telegram \u5e7f\u64ad\u9891\u9053\uff0c\u4e13\u7528\u4e8e\u5206\u4eab\u4e00\u4e9b\u6280\u672f\u4e0a\u7684\u770b\u6cd5\u3001\u4f53\u9a8c\u5fc3\u5f97\u3001\u4e00\u4e9b\u5e93 / \u9879\u76ee\u7684\u66f4\u65b0\u72b6\u51b5\u53ca\u89e3\u8bfb\uff08\u6bd4\u5982 AS\u3001support lib\uff09\uff0c\u6709\u65f6\u4e5f\u4f1a\u5206\u4eab\u4e00\u4e9b\u597d\u73a9\u7684\u4e8b\u7269\u548c\u6587\u7ae0\u94fe\u63a5", 
      "publishedAt": "2017-09-26T12:12:07.813Z", 
      "source": "web", 
      "type": "Android", 
      "url": "https://t.me/drakeets", 
      "used": true, 
      "who": "drakeet"
    }, 
    {
      "_id": "597e016c421aa90ca209c523", 
      "createdAt": "2017-07-30T23:55:24.154Z", 
      "desc": "Android\u7ec8\u7aef", 
      "publishedAt": "2017-09-21T13:27:15.675Z", 
      "source": "chrome", 
      "type": "Android", 
      "url": "https://github.com/termux/termux-app", 
      "used": true, 
      "who": "Jason"
    }, 
    {
      "_id": "597f2861421aa90ca209c527", 
      "createdAt": "2017-07-31T20:53:53.217Z", 
      "desc": "Google\u4ece API 21 \u65b0\u589e\u4e86\u63a5\u53e3 android.app.usage , \u901a\u8fc7\u8fd9\u4e2aapi\u6211\u4eec\u53ef\u4ee5\u7edf\u8ba1\u5230\u6bcf\u4e2aapp\u7684\u4f7f\u7528\u60c5\u51b5\uff0c\u542f\u52a8\u6b21\u6570\uff0c\u542f\u52a8\u65f6\u95f4\u7b49\uff0c\u4e5f\u53ef\u4ee5\u5224\u65ad\u662f\u5426\u524d\u540e\u53f0\uff0c\u6bd4\u8f83\u65b9\u4fbf\u3002", 
      "images": [
        "http://img.gank.io/c778f7da-b580-490b-961d-34706a57d326"
      ], 
      "publishedAt": "2017-09-21T13:27:15.675Z", 
      "source": "web", 
      "type": "Android", 
      "url": "http://www.jianshu.com/p/bdf47afe110d", 
      "used": true, 
      "who": "Tamic (\u7801\u5c0f\u767d)"
    }
  ]
}

Process finished with exit code 0
```

当然这些数据还需要我们进行解析、还原成原始数据，但是可以发现，与这个服务器对话的代码是如此的简单！

至于 GET 以外的请求，就需要用到 ``URLConnection`` 了，我们后面介绍。

> 下一篇文章我们了解 URI。