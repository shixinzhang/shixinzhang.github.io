---
layout: post
title: Android 热修复笔记2：代码冷启动修复
categories: Android
description: 
keywords: Android hotfix
---

记录学习 Android 热修复相关的收获。

- 代码热修复
- 代码冷启动修复
- 资源修复
- so 修复

# 代码冷启动修复

相比热修复复杂的运行环境，冷启动是一个崭新的起点，限制相对少一些，能做的也就更多。

加载补丁 dex 会遇到的一个核心问题：``unexpected dex problem.``

主要原因是：加载补丁时，如果补丁类的调用类有 ``CLASS_ISPREVERIFY``（预校验）标志，会判断调用类和补丁类是否在同一个 dex，不在的话就抛出这个异常。

而这个预校验的标签是如何打上的呢？

>在类加载时，如果一个类及其引用的类都在同一个 dex，这个类会被打上 ``CLASS_ISPREVERIFY`` 标签。

针对这个问题，主流的两种冷启动加载补丁方案：

1. （QQ 空间）在 base 包中让所有类引用一个放在单独的 dex 中的类，阻止类被打上预校验标志，然后加载补丁 dex 得到 ``dexFile`` 对象，作为参数构建一个 ``Element`` 对象，**插入到 ``dexElements`` 数组中的最前面**
2. （Tinker）提供 dex 差量包，客户端收到后将差量包与 ``classes.dex`` 合并成完整的 dex，然后加载、构建一个``Element`` 对象，**整体替换调旧的  ``dexElements`` 数组**

第一种方案简称“插桩”，第二种简称“合并全量包”。

这两种方案的共同点是：都需要侵入打包过程。

那么问题来了，``dexFile`` 和 ``dexElements`` 数组是什么呢？

>挖坑待补

![](https://user-gold-cdn.xitu.io/2018/11/12/16707451cd4a4180?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![](https://upload-images.jianshu.io/upload_images/3622938-f0d5f6ec5e255511.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/826/format/webp)

 

## dex 加载过程



# 参考资料

- 《深入探索 Android 热修复原理》
- [Android热修复原理（一）热修复框架对比和代码修复](https://blog.csdn.net/itachi85/article/details/79522200)
- [Android 热修复(全网最简单的热修复讲解)](https://www.jianshu.com/p/d17519d4952e)