---
layout: post
title: SpringBoot 笔记
categories: JavaWeb
description:  SpringBoot 笔记
keywords: 后端
---

[spring-boot 官方文档](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)


[根据依赖的内容生成 spring-boot 项目，然后用 idea 打开](https://start.spring.io/ )

-----

学习记录：


-  2019年8月5日 周一，看了几篇博客，初步了解 SpringBoot 的便捷之处，简化配置


----------

# 学习文档

知乎的 SpringBoot 话题相关不错，看精华内容


--------


SpringApplication类作为SpringBoot最基本、最核心的类，在main方法中用来启动SpringBoot项目。一般情况下，只需在main方法中使用SpringApplication.run静态方法来启动项目

Redis 作为内存缓存的形式应用到大型企业级项目中

Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
Redis支持数据的备份，即master-slave模式的数据备份。
--------------------- 
版权声明：本文为CSDN博主「xcbeyond」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xcbeyond/article/details/81116600