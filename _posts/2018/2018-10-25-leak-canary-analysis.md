---
layout: post
title: LeakCanary 原理分析
categories: Android
description: 分析 LeakCanary 原理
keywords: Android 框架 原理
---

整理一下对 LeakCanary 如何检测内存泄漏的理解。

[https://github.com/square/leakcanary](https://github.com/square/leakcanary)

- 如何检测内存泄漏
- 什么时候检测的
- Fragment 如何检测的


``ReferenceQueue``