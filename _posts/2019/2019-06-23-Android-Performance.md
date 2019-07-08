---
layout: post
title: Android 性能优化知识点
categories: Android
description: 性能优化无处不在
keywords: Android 性能 优化
---

魔鬼藏于细节之中，每一段粗心的代码都可能影响软件的性能，本文主要记录一些不那么常见的性能优化知识点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/201907072159349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9zaGl4aW4uYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70)

# 磁盘 I/O

每次打开、读写文件，操作系统需要从用户态切换到内核态，这种状态切换很消耗性能。

## 优化核心

减少磁盘 I/O 操作量，尤其是主线程的。

多使用缓存，避免重复读写。

合并读写，延迟写入。

## 知识点

### 1.避免随机读写

- 随机读写会失去预读（read-ahead）的优化效果
- 随机读写会触发 **写入放大效应**，增加延迟

>写入放大（英语：Write amplification，简称WA）是闪存和固态硬盘（SSD）中一种不良的现象，即实际写入的物理数据量是写入数据量的多倍。 
>
由于闪存在可重新写入数据前必须 先擦除，而擦除操作的 粒度 与写入操作相比低得多，执行这些操作就会**多次移动（或改写）用户数据和元数据**。因此，要改写数据，就需要读取闪存某些已使用的部分，更新它们，并写入到新的位置，如果新位置在之前已使被用过，还需连同先擦除。

>由于闪存的这种工作方式，必须擦除改写的闪存部分比新数据实际需要的大得多。此倍增效应会增加请求写入的次数，缩短SSD的寿命。
>
>[百度百科：写入放大](https://baike.baidu.com/item/%E5%86%99%E5%85%A5%E6%94%BE%E5%A4%A7/16720468?fr=aladdin)

在某些情况下，一个块（假设 512KB）里只有一个 “页” 可以使用，本来只需要写入 4KB 的数据，在 SSD 中随机读写，就需要这么多操作：

读取 A 块数据（512KB）到 B块 → B块修改 4KB → 擦除 A块（512KB） → 将 B 的数据写入 A （512KB ）

**写入放大出现场景：**

1. 手机长期使用，磁盘空间不足
2. 应用触发大量随机写

### 2.数据库

DB 文件和普通文件本质上一样，优化数据库最终也是要减少磁盘 I/O。

优化表结构、使用索引、增加缓存、调整 page-size

**慎用 ``AUTOINCREMENT``**

主键就是行号，不使用自增，被删掉的行，还可以复用。

使用它后，只能使用行号大于之前的行，而且需要另外维护一张 ``sql_sequence`` 表，导致插入性能降低。

**缓存数据库连接**

数据库在打开后，先不要关闭，在 app 退出时再关闭。

### 3.SharedPreferences 延迟写入

每调用一次 ``SharedPreferences.commit()`` 就对应一次文件的打开、关闭。

**多次 SP 操作，最好避免多次 ``commit``**

**建议用 ``apply()`` 代替 ``commit``：**

- ``apply`` 是异步操作
- ``commit`` 是同步操作

建议提前初始化 SP，初始化过程的 I/O 在主线程。

### 4.用对 Java I/O API

>https://docs.oracle.com/javase/tutorial/essential/io/streams.html

在做磁盘读写时，是否关心过使用哪个 API 性能更好呢？

选择对的 API，可能会让现有的 I/O 效率提升好几倍。

一个错误的例子是直接使用 ``FileOutputStream`` 作为参数传递给 ``ObjectOutputStream``:

```
    public void write(String file, Object data) {
        ObjectOutputStream os = null;
        try {
            os = new ObjectOutputStream(openFileOutput(file, MODE_PRIVATE));
            os.writeObject(data);
            os.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            IOUtil.closeQuietly(os);
        }
    }
```

这样在数据是一个列表时，可能会 I/O 多次。

建议使用一个 buffer 包装一层（比如 ``ByteArrayOutputStream``, ``BufferedOutputStream`` ），先把数据写到缓存区，然后再写入文件：

```
    public void write(String file, Object data) {
        ObjectOutputStream os = null;
        ByteArrayOutputStream baos;
        try {
            // 先写到缓存区
            baos = new ByteArrayOutputStream();
            os = new ObjectOutputStream(baos);
            os.writeObject(data);
            os.flush();
            
            //一次性写到磁盘
            FileOutputStream fos = openFileOutput(file, MODE_PRIVATE);
            baos.writeTo(fos);
            baos.flush();
            fos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //...
        }
    }
```

### 5.Buffer 使用合适的大小

```
byte buffer[] = new byte[1024];
byte buffer[] = new byte[8*1024];
```

Buffer 容量太大，会导致申请 Buffer 的时间变长；太小会导致 I/O 太频繁。结合要写入的数据大小，适度提高 buffer 容量。

推荐 buffer 容量为 8KB，Java 默认的容量大小就是这个。

**确定 Buffer 大小：**

1. 不能大于文件大小
2. 根据文件保存所挂载目录的 块大小（block size）确定

2.``decode`` 图片优先使用 ``BitmapFactory.decodeStream()`` 并且使用 ``BufferInputStream`` 做缓存，降低读取磁盘次数，减少耗时。

### 6.压缩文件的 API

- ``ZipFile``
- ``ZipInputStream``

``ZipFile`` 优势场景：

1. 文件已在磁盘上
2. 须全部解压
3. 随机访问

其他场景使用 ``ZipInputStream``。

### 7.解码图片使用 ``decodeStream`` 优于 ``decodeFile``

``BitmapFactory`` 解码图片时，决定写磁盘次数的是调用 native 方法 ``nativeDecodeStream`` 的次数，普通文件流会调用多次，而缓存文件流则会少很多。

``BitmapFactory.decodeFile()`` 生成的文件流是 ``FileInputStream``，无法修改；``BitmapFactory.decodeStream()`` 可以传递一个 ``BufferedInputStream`` 进去：

```
BufferedInputStream  bis = new BufferedInputStream(new FileInputStream(filePath))

Bitmap bitmap = BitmapFactory.decodeStream(bis, null, ops);
```

## 工具

- Systrace，发现主线程 I/O 或者 I/O 操作耗时过长
- STRICTMODE，发现主线程 I/O

# 内存

- 常驻内存问题（图片）
- 泄漏问题
- GC 问题（GC for Alloc）


## 图片加载到内存是如何计算占用内存大小的，有什么优化策略？

- 磁盘大小是在磁盘上占用的空间
- 内存大小是加载到内存中占用的内存大小

**图片内存大小：高度 * 图片宽度 * 一个像素占用的内存大小**

一个像素占用的内存大小，涉及到图片的编码格式。

编码格式及占用内存如下：

- ``ALPHA_8``: 1，每个像素只要一个字节，因为没编码颜色信息，只有 alpha 通道
- ``RGB_565``: 2，每个像素要 2 字节，只编码 RGB 通道
- ``ARGB_4444``: 2，每个像素要 2 字节，不过被遗弃了，建议使用下面这个
- ``ARGB_8888``: 4，每个像素要 4 字节，质量最好的一个，默认是这种

> 定义在 ``Bitmap.Config`` 中

在 ``BitmapFactory.decodeResourceStream()`` 中可以看到，放在 res 目录下的图片，在不同屏幕密度的手机和不同的目录下，会对图片做不同程度的缩放，最终影响占用内存大小。

```
     public static Bitmap decodeResourceStream(Resources res, TypedValue value,
            InputStream is, Rect pad, Options opts) {

        if (opts == null) {
            opts = new Options();
        }

        if (opts.inDensity == 0 && value != null) {
            final int density = value.density;
            if (density == TypedValue.DENSITY_DEFAULT) {
                opts.inDensity = DisplayMetrics.DENSITY_DEFAULT;
            } else if (density != TypedValue.DENSITY_NONE) {
                opts.inDensity = density;
            }
        }
        
        if (opts.inTargetDensity == 0 && res != null) {
            opts.inTargetDensity = res.getDisplayMetrics().densityDpi;
        }
        
        return decodeStream(is, pad, opts);
    }
```

缩放后图片的宽度 = 原图宽度 * （设备 dpi / 目录 dpi），高度也是如此。

目录 dpi 对应的密度：

![](https://upload-images.jianshu.io/upload_images/1924341-8b93a626fc35a76d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**可以看到，图片放置的目录密度越高，缩放后图片分辨率相对越小，所以我们开发中尽量将图片放置到高 dpi 的目录。**

- LruCache
- DiskLruCache
- ``BlobCache`` P57

# 网络

# CPU

# 电池

# Thanks

- 《Android 移动性能实战》
- [百度百科：写入放大](https://baike.baidu.com/item/%E5%86%99%E5%85%A5%E6%94%BE%E5%A4%A7/16720468?fr=aladdin)
- https://www.cnblogs.com/dasusu/p/9789389.html
- https://www.cnblogs.com/popfisher/p/6770018.html