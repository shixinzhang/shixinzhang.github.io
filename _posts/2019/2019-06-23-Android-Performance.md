---
layout: post
title: 不那么常见的 Android 性能优化知识点
categories: Android
description: 性能优化无处不在
keywords: Android 性能 优化
---

魔鬼藏于细节之中，每一段粗心的代码都可能影响软件的性能，本文主要记录一些不那么常见的性能优化知识点。

# 磁盘


1.``decode`` 图片优先使用 ``BitmapFactory.decodeStream()`` 并且使用 ``BufferInputStream`` 做缓存，降低读取磁盘次数，减少耗时。

2.慎用 ``AUTOINCREMENT``

主键就是行号，不使用自增，被删掉的行，还可以复用。

使用它后，只能使用行号大于之前的行，而且需要另外维护一张 ``sql_sequence`` 表，导致插入性能降低。

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

- https://www.cnblogs.com/dasusu/p/9789389.html
- https://www.cnblogs.com/popfisher/p/6770018.html