---
layout: post
title: 极客时间《许世伟的架构课》笔记
categories: ComputerFoundation
description: 笔记
keywords: 基础，架构
---

极客时间[《许世伟的架构课》](https://time.geekbang.org/column/article/98406) 


## 内存

谈内存管理，需要谈清楚两个核心问题：

- 如何分配内存（给运行中的软件，避免它们发生资源争抢）；
- 如何运行外置存储（比如硬盘）上的软件？


我们分别就在实模式下和保护模式下的内存管理进行了讨论。

## 系统调用

许世伟的架构课08

着重介绍的是操作系统的系统调用背后的实现机理。通过系统调用这个机制，我们很好地实现了操作系统和应用软件的隔离性和安全性，同时仍然保证了极好的执行性能。


## 文件系统

格式化最重要的是标记分区的文件系统格式（用来告诉别人这个分区是数据是怎么组织的），并且生成文件系统的根目录。

