---
layout: post
title: Android APM 实现原理
categories: Android
description: 性能优化无处不在
keywords: Android 性能 监控 优化
---

记录 Android APM 监控的一些原理，包括：

- I/O 监控
- 内存监控
- 网络监控
- FPS 监控
- 绘制监控
- 启动监控

# 性能指标

1.流畅度：FPS 均值大于 30，最小值大于 24

>Intel 研究表明，动画大于 24FPS 是用户不感觉到卡顿的最低标准.