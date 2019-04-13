---
layout: post
title: Linux 系统学习
categories: Linux
description: 记录下 Linux 系统学习笔记
keywords: Linux 操作系统
---

# 文件管理

**文件 IO**

- open
- read
- write


**IO 多路复用**

- select()
- poll()
- 二者区别

## 高级 IO

# 进程管理

- 进程体系及进程 ID
- 运行新进程
- 终止进程
- 进程调度

# 线程管理

- 线程模式
- 互斥
- 死锁
- Pthreads


# 内存管理

- 固定分区
- 动态分区
- 分段机制
- 分页机制

## 分页机制

- 内存划分
- CPU 运作模式

### 内存映射

物理内存和虚拟内存如何映射

- 页表的翻译
- 物理页面的分配和释放
- 伙伴系统算法（内存碎片化问题）

# 信号