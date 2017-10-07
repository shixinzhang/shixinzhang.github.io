---
layout: post
title: Java 网络编程之 URI 类
categories: Java 网络编程
description: URI 类的使用
keywords: Java 网络编程 URI
---

有些情况下 URI 比 URL 更合适，这篇文章来了解 URL 他哥 URI。

URI 是对 URL 的抽象，它包括两部分：

1. 统一资源定位符 URL (Uniform Resource Locators)
2. 统一资源名 URN (Uniform Resource Name)


实际中使用的 URI 大多是 URL，但大多数规范和标准都是用 URI 定义的。

URL 主要用于从服务器下载内容；URI 主要用来标识一个资源。它们可以互相转换，``toURL()``, ``toURI()``。


**Java 中的 URI 类提供了比 URL 类更精确、更规范的行为，在编码等操作时，首选 URI；如果需要把 URL 存储在一个 map 里，最好也用 URI，因为它的 ``equals()`` 方法不会阻塞。**


# URI 类

简而言之，URL 对象是对应网络获取的应用层协议的一个表示；而 URI 对象纯粹用于解析和处理字符串。

URI 类没有网络获取功能。

## 构造一个 URL

通过传入不同的参数可以构造：


![](https://ws1.sinaimg.cn/large/b1aad299gy1fk9fxeh1prj21m20hb7jr.jpg)

## URI 的各部分

有模式的 URI 是绝对 URI；没有模式的是相对 URI。

```
public String getAuthority()
public String getFragment()
public String getHost()
public String getPath()
public String getPort()
public String getQuery()
public String getUserInfo()
```

这些方法会返回解码后的 URI 部分，换句话说，百分号转义的字符会改为它们实际表示的字符，比如 %3C 会还原为 <。

如果希望得到 URI 原始的编码部分，可以使用这 5 个 ``getRawXXX()`` 方法：

```
public String getRawAutority()
public String getRawFragment()
public String getRawPath()
public String getRawQuery()
public String getRawUserInfo()
```

URI 的这些方法可以用于获取任意自定义的、语法正确的 URI，而不仅限于已知协议的那些。

**不透明 URI 与 分层 URI**：

- 不透明URI：scheme-specific-part 组件不是以正斜杠(/)起始的，如mailto:sxzhang2016@163.com
- 分层URI：scheme-specific-part 组件是以正斜杠(/)起始的，如 http://shixinzhang.top

由于不透明 URI 无需进行分解操作，因此不会对 scheme-specific-part 组件进行有效性验证。

测试一下这些方法：

```
/**
 * URI 的各个获取方法
 */
private static void testUriGetter() {
    try {
        URI uri = new URI("zsx://shixinzhang.top:8080/categories/android?page=3#3
");

        System.out.println(uri + " isOpaque : " + uri.isOpaque());

        print("scheme: " + uri.getScheme());
        print("fragment ID is: " + uri.getFragment());
        if (uri.isOpaque()){    //不透明，不是以 / 开始
            print("scheme specific part is: " + uri.getSchemeSpecificPart());
        }else { //不是透明的，就是分层的
            uri = uri.parseServerAuthority();
            print("host is: " + uri.getHost());
            print("user info is: " + uri.getUserInfo());
            print("port is: " + uri.getPort());
            print("path is: " + uri.getPath());
            print("query string is: " + uri.getQuery());
        }
    } catch (URISyntaxException e) {
        e.printStackTrace();
    }
}

private static void print(@NonNull final String s) {
    System.out.println("The " + s);
}
```

运行结果：

```
zsx://shixinzhang.top:8080/categories/android?page=3#3 isOpaque : false
The scheme: zsx
The fragment ID is: 3
The host is: shixinzhang.top
The user info is: null
The port is: 8080
The path is: /categories/android
The query string is: page=3

Process finished with exit code 0
```


## 解析绝对/相对 URI

URI 类提供了几个用法用于获取两个 URI 中的绝对 URI 和相对 URI：

```
public URI resolve(URI uri)
public URI resolve(String uri)
public URI relative(URI uri)
```

其中 ``resolve()`` 的实现如下：

```
public URI resolve(URI uri) {
    return resolve(this, uri);
}
private static URI resolve(URI base, URI child) {
    // check if child if opaque first so that NPE is thrown
    // if child is null.
    if (child.isOpaque() || base.isOpaque())    //如果有一个是不透明的，直接返回 child
        return child;

    // 5.2 (2): Reference to current document (lone fragment)
    if ((child.scheme == null) && (child.authority == null)
        && child.path.equals("") && (child.fragment != null)
        && (child.query == null)) {    //如果第二个 URI 只有 fragment
        if ((base.fragment != null)
            && child.fragment.equals(base.fragment)) {
            return base;
        }
        URI ru = new URI();
        ru.scheme = base.scheme;
        ru.authority = base.authority;
        ru.userInfo = base.userInfo;
        ru.host = base.host;
        ru.port = base.port;
        ru.path = base.path;
        ru.fragment = child.fragment;    //就直接把第二个 URI 的 fragment 拼到第一个上
        ru.query = base.query;
        return ru;
    }

    // 5.2 (3): Child is absolute
    if (child.scheme != null)    //如果第二个 URI 有 scheme，就直接返回第二个
        return child;

    URI ru = new URI();             //没有 scheme，就使用第一个的 scheme
    ru.scheme = base.scheme;
    ru.query = child.query;
    ru.fragment = child.fragment;

    // 5.2 (4): Authority
    if (child.authority == null) {    //如果第二个 URI 的 authority 为空，就使用第一个的
        ru.authority = base.authority;
        ru.host = base.host;
        ru.userInfo = base.userInfo;
        ru.port = base.port;

        if (child.path == null || child.path.isEmpty()) {
            // This is an addtional path from RFC 3986 RI, which fixes following RFC 2396
            // "normal" examples:
            // Base: http://a/b/c/d;p?q
            //   "?y" = "http://a/b/c/d;p?y"
            //   ""   = "http://a/b/c/d;p?q"
            // http://b/25897693
            ru.path = base.path;
            ru.query = child.query != null ? child.query : base.query;
        } else if ((child.path.length() > 0) && (child.path.charAt(0) == '/')) {
            // 5.2 (5): Child path is absolute
            //
            // There is an additional step from RFC 3986 RI, requiring to remove dots for
            // absolute path as well.
            // http://b/25897693
            ru.path = normalize(child.path, true);
        } else {
            // 5.2 (6): Resolve relative path
            ru.path = resolvePath(base.path, child.path, base.isAbsolute());
        }
    } else {
        ru.authority = child.authority;
        ru.host = child.host;
        ru.userInfo = child.userInfo;
        ru.host = child.host;
        ru.port = child.port;
        ru.path = child.path;
    }

    // 5.2 (7): Recombine (nothing to do here)
    return ru;
}
```

可以看到，``resolve()`` 方法做的就是尽可能多地将参数 URI 和调用 URI 组合，如果参数 URI 信息比较完全，就返回参数 URI；如果参数 URI 是个相对 URI，就拼到第一个 URI 上然后再返回。

举个例子，第二个参数是完整 URI：

```
private static void testUriResolve() {
    try {
        URI firstUri = new URI("http://www.shixinzhang.top/");
        URI secondUri = new URI("http://shixinzhang.top/images/logo.png");

        URI resolve = firstUri.resolve(secondUri);
        System.out.println(resolve);
    } catch (URISyntaxException e) {
        e.printStackTrace();
    }
}
```

第二个比较完整，会直接返回第二个：

```
http://shixinzhang.top/images/logo.png

Process finished with exit code 0
```

第二个参数是相对 URI：

```
private static void testUriResolve() {
    try {
        URI firstUri = new URI("http://www.shixinzhang.top/");
        URI secondUri = new URI("images/logo.png");

        URI resolve = firstUri.resolve(secondUri);
        System.out.println(resolve);
    } catch (URISyntaxException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
http://www.shixinzhang.top/images/logo.png

Process finished with exit code 0
```

``relative()`` 的实现：

```
public URI relativize(URI uri) {
    return relativize(this, uri);
}
private static URI relativize(URI base, URI child) {
    // check if child if opaque first so that NPE is thrown
    // if child is null.
    if (child.isOpaque() || base.isOpaque())    //有一个是不透明的，就返回第二个 URI
        return child;
    if (!equalIgnoringCase(base.scheme, child.scheme)
        || !equal(base.authority, child.authority))
        return child;    //如果 scheme 或者 authority 有一个不相同，就返回第二个

    String bp = normalize(base.path);
    String cp = normalize(child.path);
    if (!bp.equals(cp)) {
        // Android-changed: The original OpenJdk implementation would append a trailing slash
        // to paths like "/a/b" before relativizing them. This would relativize /a/b/c to
        // "/c" against "/a/b" the android implementation did not do this. It would assume that
        // "b" wasn't a directory and relativize the path to "/b/c". The spec is pretty vague
        // about this but this change is being made because we have several tests that expect
        // this behaviour.
        if (bp.indexOf('/') != -1) {
            bp = bp.substring(0, bp.lastIndexOf('/') + 1);
        }

        if (!cp.startsWith(bp))
            return child;
    }

    URI v = new URI();
    v.path = cp.substring(bp.length());
    v.query = child.query;
    v.fragment = child.fragment;
    return v;
}
```

使用例子：

```
private static void testUriRelativize() {

    try {
        URI firstUri = new URI("http://www.shixinzhang.top/images/logo.png");
        URI secondUri = new URI("http://www.shixinzhang.top");

        URI relativize = secondUri.relativize(firstUri);
        System.out.println(relativize);
    } catch (URISyntaxException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
images/logo.png

Process finished with exit code 0
```

可以看到，``relativize()`` 的作用是对比调用 URI 和参数 URI 中，参数 URI 多余的相对 URI，所以调用 URI 一般是短一些的。


## 相等性和比较

``equals()`` 方法的几个比较条件：

- 相等的 URI 必须都是层次的或者都是不透明的
- 比较 scheme 和 authority 时不考虑大小写
- 其余部分要区分大小写
- 转移字符在比较前不解码，``http://shixinzhang.top/A`` 和 ``http://shixinzhang.top/%41`` 不相等

``hashCode()`` 方法与相等性是一致的。

URI 实现了 ``Comparable``，因此 URI 可以排序，基于各个部分的字符串比较结果，按一下顺序进行排序：


![](https://ws1.sinaimg.cn/large/b1aad299gy1fk9fy425l2j21vy0ugx5s.jpg)

## 字符串表示

URI 类有两个方法转成 String：``toString()`` 和 ``toASCIIString()``:

```
private static void testUriToString() {
    try {
        URI firstUri = new URI("https://shixinzhang.top/2017/01/15/java-url-class/#创建新的-url");
        System.out.println(firstUri.toString());
        System.out.println(firstUri.toASCIIString());
    } catch (URISyntaxException e) {
        e.printStackTrace();
    }
}
```

运行结果：

```
https://shixinzhang.top/2017/01/15/java-url-class/#创建新的-url
https://shixinzhang.top/2017/01/15/java-url-class/#%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84-url

Process finished with exit code 0
```

对比结果就可以看出来它俩的区别：

- ``toString()`` 直接返回 URI 未编码的字符串形式，不会将非 ASCII 码的字符转义
- ``toASCIIString()`` 会返回 URI 编码的字符串形式，会将非 ASCII 码字符转义