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

# 网络

# CPU

# 电池