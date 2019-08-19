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

------------

@所有人   app专项优化计划，

- app启动阶段的网络合并
- 页面静止情况下cpu运行情况（展示运行事件的堆栈）
- 模块做成熟，就可以加入现有的二方库给其他app来使用，适合的话最后会加入到apm线上平台来监控
- 这是一个很好的锻炼机会，可以了解不少android底层知识

# 性能优化分享实践

- [极客时间 Android 高手课 代码](https://github.com/AndroidAdvanceWithGeektime)
- [手淘启动优化](https://mp.weixin.qq.com/s/PiqnHezWKWUU0byEhrboRg)
- [手Q Android线程死锁监控与自动化分析实践](https://cloud.tencent.com/developer/article/1064396)
- [pf分享的安卓网络超时原因分析文章，这个人的专栏挺不错的](https://zhuanlan.zhihu.com/p/31640388)
- [Android的死机、重启问题分析方法](http://www.itkeyword.com/doc/816470999526841x714)
- [总结了很多性能优化相关的知识点](https://baiqiantao.github.io/%E4%BC%98%E5%8C%96/Efeiey/)

# 性能指标

1.流畅度：FPS 均值大于 30，最小值大于 24

>Intel 研究表明，动画大于 24FPS 是用户不感觉到卡顿的最低标准.


# 网络监控


解决了什么问题，优化了什么 ？
不影响业务性能的情况下做监控、统计
okhttp 请求监控，后面沉淀成二方库

目的：统计网络请求是否都集中在某一阶段
最终目的：合理分配请求的优先级，业务展示相关的优先

数据：某一时刻请求的数量、请求所属业务、请求的 URL

优化点：合并请求、优先级调度

怎么做：了解请求的流程，在关键点做拦截、统计