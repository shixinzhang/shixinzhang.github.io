---
layout: post
title: AndroidFramework 之系统启动到应用启动、消息循环
categories: Android
description: 学习安卓源码笔记
keywords: Android
---


# 系统启动流程

init.rc -> Zygote ->

# 应用启动流程

# 消息循环

- 消息循环的创建
- 消息的处理
- 消息的发送

## 消息循环创建过程

Java 层 Looper.prepare() -> new MessageQueue() -> C++ 层 NativeMessageQueue -> C++ 层 Looper -> 创建管道，保存读写文件描述符 ->.epoll 实例监听这个管道

