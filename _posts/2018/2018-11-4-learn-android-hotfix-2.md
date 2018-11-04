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


 

## dex 加载过程



# 参考资料

主要学习自《深入探索 Android 热修复原理》