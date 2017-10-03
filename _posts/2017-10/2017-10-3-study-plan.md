---
layout: post
title: 这个阶段的一些学习目标
categories: Plan
description: 这个阶段的一些学习目标
keywords: Android, Java,进阶,学习目标
---


[TOC]

>原文地址：http://blog.csdn.net/u011240877
  
为了方便读者阅读以及自己回顾，总结写过的文章和一些想要写的文章目录如下：


#1.JavaSE

**这个人的 JavaSE 专栏不错：http://www.cnblogs.com/chenssy/category/525010.html**

[Java 解惑：Comparable 和 Comparator 的区别](http://blog.csdn.net/u011240877/article/details/53399019)

[Java 解惑：Random 种子的作用、含参与不含参构造函数区别](http://blog.csdn.net/u011240877/article/details/52971166)

[Java 解惑：CharSequence 与 String 的区别](http://blog.csdn.net/u011240877/article/details/47423567)

[ Java 解惑：String 为什么不可修改](http://blog.csdn.net/u011240877/article/details/47256579)


**Java 关键字：final**

**Java 关键字：static**

**Java 内部类、静态内部类、匿名内部类**
 - 重点是classloader load class过程中创建该实例会不会有区别
 - 成员变量是load这个class就会被load，内部类是看需要用到，再去加载吧
 - 所以说啊，内部类能懒加载，外部类不能

## 多态

- 深入理解：抽象类
- 深入理解：接口
 - 为什么成员变量、方法必须是 public static final http://blog.csdn.net/linfeng24/article/details/38406625
- 抽象类与接口如何选择？


## 泛型

- [深入理解 Java 泛型](http://blog.csdn.net/u011240877/article/details/53545041)
- 再次探讨 Java 泛型与类型擦除

## 注解

- [什么是注解以及运行时注解的使用](http://blog.csdn.net/u011240877/article/details/74486834)
- [使用编译时注解简单实现类似 ButterKnife 的效果](http://blog.csdn.net/u011240877/article/details/74490201)
- 使用 SupportAnnotation 简化你的开发 
 - https://blog.mindorks.com/improve-your-android-coding-through-annotations-26b3273c137a


## 反射

- [深入理解 Java 反射：Class （反射的入口）
](http://blog.csdn.net/u011240877/article/details/54604146)
- [深入理解 Java 反射：Field （成员变量）](http://blog.csdn.net/u011240877/article/details/54604212)
- [深入理解 Java 反射：Method （成员方法）](http://blog.csdn.net/u011240877/article/details/54604224)
- 深入理解 Java 反射：constructor
- 反射的实现原理（虚拟机如何实现的）

## 集合

- [ Java 集合源码解析（1）：Iterator](http://blog.csdn.net/u011240877/article/details/52743564)
- [ Java 集合源码解析（2）：ListIterator](http://blog.csdn.net/u011240877/article/details/52752589)
- [Java 集合深入理解（3）：Collection](http://blog.csdn.net/u011240877/article/details/52773577)
- [ Java 集合深入理解（4）：List<E> 接口](http://blog.csdn.net/u011240877/article/details/52802849)
- [ Java 集合深入理解（5）：AbstractCollection](http://blog.csdn.net/u011240877/article/details/52829912)
- [ Java 集合深入理解（6）：AbstractList](http://blog.csdn.net/u011240877/article/details/52834074)
- [Java 集合深入理解（7）：ArrayList](http://blog.csdn.net/u011240877/article/details/52853989)
- [Java 集合深入理解（8）：AbstractSequentialList](http://blog.csdn.net/u011240877/article/details/52854681)
- [ Java 集合深入理解（9）：Queue 队列](http://blog.csdn.net/u011240877/article/details/52860924)
- [ Java 集合深入理解（10）：Deque 双端队列](http://blog.csdn.net/u011240877/article/details/52865173)
- [ Java 集合深入理解（11）：LinkedList](http://blog.csdn.net/u011240877/article/details/52876543)
- [ Java 集合深入理解（12）：古老的 Vector](http://blog.csdn.net/u011240877/article/details/52900893)
- [ Java 集合深入理解（13）：Stack 栈](http://blog.csdn.net/u011240877/article/details/52901274)
- [ Java 集合深入理解（14）：Map 概述](http://blog.csdn.net/u011240877/article/details/52929523)
- [ Java 集合深入理解（15）：AbstractMap](http://blog.csdn.net/u011240877/article/details/52949046)
- [ Java 集合深入理解（16）：HashMap 主要特点和关键方法源码解读](http://blog.csdn.net/u011240877/article/details/53351188)
- [ Java 集合深入理解（17）：HashMap 在 JDK 1.8 后新增的红黑树结构](http://blog.csdn.net/u011240877/article/details/53358305)
- Java 集合深入理解（18）：LinkedHashMap
- Java 集合深入理解（19）：TreeMap
- Java 集合深入理解（20）：ConcurrentHashMap
- Java 集合深入理解（21）：Set
- SparseArray（SparseIntArray）
 - https://blog.mindorks.com/android-app-optimization-using-arraymap-and-sparsearray-f2b4e2e3dc47
- ArrayMap
- Java 集合框架设计思想总结

## 异常

- try catch finally，try里有return，finally还执行么？
- Excption与Error包结构。OOM你遇到过哪些情况，SOF你遇到过哪些情况。

**IO / NIO**

## Java 8 特性

- lambda
- Optional
- Stream

#2.数据结构与算法

## 数据结构：

- [ 重温数据结构：哈希 哈希函数 哈希表](http://blog.csdn.net/u011240877/article/details/52940469)
- [ 重温数据结构：树 及 Java 实现](http://blog.csdn.net/u011240877/article/details/53193877)
- [ 重温数据结构：二叉树的常见方法及三种遍历方式 Java 实现](http://blog.csdn.net/u011240877/article/details/53193918)
- [ 重温数据结构：二叉排序树的查找、插入、删除](http://blog.csdn.net/u011240877/article/details/53242179)
- [ 重温数据结构：深入理解红黑树](http://blog.csdn.net/u011240877/article/details/53329023)
- [ 怎么判断一个序列是不是堆？](http://blog.csdn.net/u011240877/article/details/47706923)
- 有向无环图的解释

## 算法：

- [ 使用并查集和优先队列实现 Kruskal 算法](http://blog.csdn.net/u011240877/article/details/41630575)
- 快速排序的实现，时间/空间复杂度
- 堆排序的实现，时间/空间复杂度
- 链表反转
- 海量数据 字典查找
- 字符串匹配
- 两个不重复的数组集合中，求共同的元素。
 - 扩展，海量数据，内存中放不下，怎么求出

>常见排序算法的思路、实现与效率对比
>常见查找算法的思路与实现

#3.设计模式

- [ 设计模式六大原则： 一个萝卜一个坑 -- 单一职责原则](http://blog.csdn.net/u011240877/article/details/52177033)
- [ 设计模式六大原则： 一国两制 -- 开放封闭原则](http://blog.csdn.net/u011240877/article/details/52187631)
- [ 设计模式六大原则： 狸猫换太子 -- 里氏替换原则](http://blog.csdn.net/u011240877/article/details/52187810)
- [ 设计模式六大原则： 老板是如何减轻负担的 -- 依赖倒置原则](http://blog.csdn.net/u011240877/article/details/52194373)
- [ 设计模式六大原则： 辅导班的因材施教 -- 接口隔离原则](http://blog.csdn.net/u011240877/article/details/52213659)
- 设计模式六大原则： 迪米特原则
- [ Java 实现多种单例模式 SingletonPattern](http://blog.csdn.net/u011240877/article/details/46988219)
- [ 代理模式：女朋友这么漂亮，你缺经纪人吗？](http://blog.csdn.net/u011240877/article/details/52264283)
- [ 动态代理：1 个经纪人如何代理 N 个明星](http://blog.csdn.net/u011240877/article/details/52334547)
- [ 策略模式：网络小说的固定套路](http://blog.csdn.net/u011240877/article/details/52346671)
- [ Android 中的那些策略模式](http://blog.csdn.net/u011240877/article/details/52493408)
- [ 适配器模式 : 农村小伙娶乌克兰美女语言不通 翻译软件立功](http://blog.csdn.net/u011240877/article/details/52601040)
- [ 观察者模式 : 一支穿云箭，千军万马来相见](http://blog.csdn.net/u011240877/article/details/52683558)
- [ 最熟悉的陌生人：ListView 中的观察者模式](http://blog.csdn.net/u011240877/article/details/52683711)
- [ 变种 Builder 模式：优雅的对象构建方式](http://blog.csdn.net/u011240877/article/details/53248917)
- 简单工厂模式
- 抽象工厂模式
- 工厂方法模式
- 装饰模式与 Java I/O 流
- 组合模式
- 迭代器模式
- 外观模式
- [ 23 种经典设计模式 UML 类图汇总](http://blog.csdn.net/u011240877/article/details/45381071)

#4.架构设计

https://github.com/googlesamples/android-architecture
https://github.com/android10/Android-CleanArchitecture

- 抽象类和接口的区别
 - 各自适合的使用场景
- MVC 的思路与实现
- MVP 的思路与实现
- MVVM 的思路与实现

## 搭建自己的框架：

- 基类
 - ActionBar
- 网络
- 图片加载
- 缓存
- Hybrid
- 工具类
- UBT 统计（埋点）
- 分享
- 更新
- 错误捕获处理
 - [How to setup ACRA, an Android Application Crash Tracking system, on your own host](https://inthecheesefactory.com/blog/how-to-install-and-use-acra-android/en)
- 日志管理上传
- 推送

#5.数据通信与网络

## 理论知识

- [ Ethernet 和 Internet 的区别是什么？](http://blog.csdn.net/u011240877/article/details/52168170)
- URL 和 URI 的区别？
- [ HTTP 状态代码 201 304 404 500 等代表的含义](http://blog.csdn.net/u011240877/article/details/46604973)
- 深入理解 HTTP 协议
- 深入理解 TCP/IP 协议

参考下这个：https://github.com/LiushuiXiaoxia/AndroidHttp

**从发出请求到响应的整个过程**


## Java 网络编程

## Android 网络编程


- 看那个下载的快传代码
- 断点下载
- 文件上传


# 6.并发编程/多线程

## Java 并发：

- [ 并发编程：全面认识 Thread](http://blog.csdn.net/u011240877/article/details/57202704)
- [ 并发编程：认识并发编程的利与弊](http://blog.csdn.net/u011240877/article/details/58756137)
- 并发编程：深入理解 synchronized
- 并发编程：深入理解 volatile
- 并发编程：CAS 介绍
- 并发编程：Java 中的显式锁 Lock
 - ReentrantLock 的实现
 - synchronized与lock
- 并发编程：线程通信的几种方式
- 并发编程：ConCurrentHashMap实现
- 并发编程：死锁
 - 怎么避免死锁
- 并发编程：手写生产者/消费者模式
- [并发编程：线程池的使用与执行流程](http://blog.csdn.net/u011240877/article/details/73440993)
- [ 并发编程：Java 7 种阻塞队列源码分析（上）](http://blog.csdn.net/u011240877/article/details/73612930)
- [并发编程：Java 7 种阻塞队列源码分析（下）](http://blog.csdn.net/u011240877/article/details/73742407)

## Android 多线程：

- [源码解读 Android 消息机制（ Message MessageQueue Handler Looper）](http://blog.csdn.net/u011240877/article/details/72892321)
- [HandlerThread 使用场景及源码解析](http://blog.csdn.net/u011240877/article/details/72905631)
- [IntentService 使用及源码解析](http://blog.csdn.net/u011240877/article/details/72972610)
- Android 多线程：AsyncTask


#7.Android

## Android 基础问题：

- [ Activity 生命周期一次搞定](http://blog.csdn.net/u011240877/article/details/45026081)
- [ Activity间跳转时的效果设计，页面切换效果](http://blog.csdn.net/u011240877/article/details/44834621)
- [ Android 自定义按钮状态背景](http://blog.csdn.net/u011240877/article/details/44831047)
- [ Android自定义组合控件的过程](http://blog.csdn.net/u011240877/article/details/44781351)
- [ Android ContentProvider 之联系人数据库及操作](http://blog.csdn.net/u011240877/article/details/44594645)
- [ Android 存储之 SharedPreferences](http://blog.csdn.net/u011240877/article/details/44477747)
- [ Fragment 间传递数据 Communicating with Other Fragments ](http://blog.csdn.net/u011240877/article/details/51012830)
- [ Android Focusable in Touch Mode 介绍](http://blog.csdn.net/u011240877/article/details/52672738)
- [ Intent 传递数据和 Bundle 传递数据的区别](http://blog.csdn.net/u011240877/article/details/47658893)
- [ Android 使用 Intent 打开电话、短信、邮箱、本地文件等系统应用程序整理大全](http://blog.csdn.net/u011240877/article/details/46460029)
- [ Android 应用中如何调用系统闹钟及日历](http://blog.csdn.net/u011240877/article/details/40454703)
- [ 代码中修改 TextView 的 DrawableLeft 图片](http://blog.csdn.net/u011240877/article/details/46349263)
- [ ScrollView ListView 滚动冲突、显示不全 解决办法](http://blog.csdn.net/u011240877/article/details/46051669)
- [ Android开发之使用VideoView实现视频的横屏播放、去除边框](http://blog.csdn.net/u011240877/article/details/45601061)
- [ Android 实现 首次点击返回键提示信息，第二次点击退出应用](http://blog.csdn.net/u011240877/article/details/46389699)

## Android 进阶：

- [Android 进阶1：Activity 的生命周期](http://blog.csdn.net/u011240877/article/details/71076342)
- [Android 进阶2：Activity 的 Task 与启动模式](http://blog.csdn.net/u011240877/article/details/71082720)
- [Android 进阶3：Intent 与 IntentFilter 匹配规则](http://blog.csdn.net/u011240877/article/details/71305797)
- [Android 进阶4：Service 的一些细节](http://blog.csdn.net/u011240877/article/details/72654743)
- [Android 进阶5：Activity 的继承结构]()
- [Android 进阶6：两种序列化方式 Serializable 和 Parcelable](http://blog.csdn.net/u011240877/article/details/72455715)

## Android 跨进程通信：

- [Android 进阶：进程通信之 AIDL 的使用](http://blog.csdn.net/u011240877/article/details/72765136) 
- [ Android 进阶：进程通信之 AIDL 解析](http://blog.csdn.net/u011240877/article/details/72825706)
- [ Android 进阶：进程通信之 Binder 机制浅析](http://blog.csdn.net/u011240877/article/details/72801425)
- [ Android 进阶：进程通信之 Messenger 使用与解析](http://blog.csdn.net/u011240877/article/details/72836178) 
- [Android 进阶：进程通信之 ContentProvider 内容提供者](http://blog.csdn.net/u011240877/article/details/72848608) 
- [ Android 进阶：进程通信之 Socket （顺便回顾 TCP UDP）](http://blog.csdn.net/u011240877/article/details/72860483)
- [Android 进阶：几种进程通信方式的对比总结](http://blog.csdn.net/u011240877/article/details/72863432)

## Android 事件总线：

- EventBus 
 - [ Android 框架学习：EventBus 3.0 的特点与如何使用](http://blog.csdn.net/u011240877/article/details/73015939)
 - [ Android 框架学习：源码分析 EventBus 3.0 如何实现事件总线](http://blog.csdn.net/u011240877/article/details/73196808)
- Otto 的使用与解析
- RxBus 的使用与解析
- 三种事件总线对比分析

## Android 四大组件：

BroadcastReceiver 细节，什么时候使用
ContentProvider 细节，什么时候使用

- [深入理解 Android 四大组件：Activity 的启动流程](
- [深入理解 Android 四大组件：Service 的启动流程](
- [深入理解 Android 四大组件：ContentProvider 的启动流程](
- [深入理解 Android 四大组件：BroadcastReceiver 的启动流程](

## Android 自定义 View 基础:

- Android Window 与 WindowManager
- Window 与 Activity、Dialog、Toast 的创建
- View 的生命周期
 - onFinishedInflated, onAttachedToWindow, onDetachedFromWindow 什么时候调用
 - http://www.jianshu.com/p/73f347c028e4
- View 的滑动，实现弹性滑动父容器
- View 的事件分发机制，写例子，多层的
- View 的工作过程三部曲
 - measure
 - layout
 - draw
- Android Drawable 概览
- 属性动画进阶：自定义插值器

- Canvas
- Path
- Paint
- Matrix

## Android 自定义 View 实战

自定义使用频率很高的控件：

- 无限轮播 banner
 - https://github.com/youth5201314/banner
- 状态 View（加载中、加载失败、空白）
- 下拉刷新、上拉加载
- 加载进度
- 图片选择
 - https://github.com/zhihu/Matisse
- [ Android 基于 wheelView 的自定义日期选择器(可拓展样式)](http://blog.csdn.net/u011240877/article/details/46652851)


## Android 图片加载：

- Bitmap 压缩处理

## Android 实战：

- [ 帮学长毕业设计总结：AChartEngine 创建图表的步骤](http://blog.csdn.net/u011240877/article/details/45298863)
- [ Android 实现 拍照测距 的APP](http://blog.csdn.net/u011240877/article/details/51312465)

## Android 框架使用与分析：

- okhttp3
- retrofit
- volley
- butterknife
- eventbus
- picasso
 - [为什么我重新使用Picasso加载网络图片？](http://www.jianshu.com/p/6e96dfde7c6c)
- glide
- leakcanary
- [blockcanary](https://github.com/markzhai/AndroidPerformanceMonitor)
- recyclerview

## Android 相关工具：

- [ 了解 Android Studio Live Templates , 加快开发的“咒语”](http://blog.csdn.net/u011240877/article/details/52198958)

#8.性能优化

- [ Android 性能优化：使用 Lint 优化代码、去除多余资源](http://blog.csdn.net/u011240877/article/details/54141714)
- [ Android 性能优化：使用 TraceView 找到卡顿的元凶](http://blog.csdn.net/u011240877/article/details/54347396)
- [ Android 性能优化：多线程优化](http://blog.csdn.net/u011240877/article/details/53142177)
- Android 性能优化：布局优化
- Android 性能优化：网络优化
- Android 性能优化：耗电优化


#9.Hybrid

> Hybrid 框架设计 ?

- [ Hybrid：Android 中如何获取和写入 H5 localStorage 数据](http://blog.csdn.net/u011240877/article/details/52839845)
- Hybrid 路由、导航
- Hybrid 离线缓存、更新
- Hybrid WebView 预加载
- 异常页面？

#10.跨平台

## React Native :

- [ React Native 学习：Windows 上搭建环境踩坑记录](http://blog.csdn.net/u011240877/article/details/51971528)
- [ React Native backgroundColor 的颜色值](http://blog.csdn.net/u011240877/article/details/51982207)
- [ React Native 小米（红米）手机安装失败、白屏 Failed to establish session 解决方案](http://blog.csdn.net/u011240877/article/details/51983262)
- [ React Native 集成到 Android 原生项目中踩坑记录 (Didn't find class "com.facebook.jni.IteratorHelper")](http://blog.csdn.net/u011240877/article/details/52095216)

## Weex：

- Weex 体验踩坑记录
- Weex Android 源码浅析

#11.前端

[ npm 与 package.json 快速入门](http://blog.csdn.net/u011240877/article/details/76582670)

JavaScript：

- [JavaScript 的闭包是什么](http://blog.csdn.net/u011240877/article/details/70194129)
- [JavaScript 的闭包用于什么场景](http://blog.csdn.net/u011240877/article/details/70202456)
- [原型继承链]

ECMAScript 2015:

- 块级作用域
- Promise

Node.js

#12.虚拟机

JVM

- 编译时、运行时
- 内存模型，以及分区，需要详细到每个区放什么
 - 堆里面的分区：Eden，survival from to，老年代，各自的特点
- 对象创建方法，对象的内存分配，对象的访问定位
- 垃圾回收机制的算法，收集
 - GC的两种判定方法：引用计数与引用链
 - GC的三种收集方法：标记清除、标记整理、复制算法的原理与特点，分别用在什么地方，如果让你优化收集方法，有什么思路？
 - GC收集器有哪些？CMS收集器与G1收集器的特点
 - Minor GC与Full GC分别在什么时候发生
- 几种常用的内存调试工具：jmap、jstack、jconsole
- 类加载的五个过程：加载、验证、准备、解析、初始化
- 分派：静态分派与动态分派
- Java中对象的生命周期

> JVM过去过来就问了这么些问题，没怎么变，内存模型和GC算法这块问得比较多

Dalvik 

ART

Dalvik VM, ART 和 JVM 的区别

#13.Git

- [ git 对比两个分支差异](http://blog.csdn.net/u011240877/article/details/52586664)
- [ git pull --rebase 做了什么？ 以及 Cannot rebase: You have unstaged changes 解决办法](http://blog.csdn.net/u011240877/article/details/52668807)

#14.Gradle

- [ Gradle for Android 系列：为什么 Gradle 这么火](http://blog.csdn.net/u011240877/article/details/53572264)
- [ Gradle for Android 系列：初识 Gradle 文件](http://blog.csdn.net/u011240877/article/details/53798052)
- Gradle for Android 系列：Groovy 
- Gradle for Android 系列：Android Gradle Plugin 开发

#15.Android 测试

>官方 todo-mvp 项目中为了进行UI测试引入了Espresso，为了对业务层进行单元测试引入了junit，为了生成测试mock对象引入了mockito，为了支撑mockito又引入了dexmaker，hamcrest的引入使得测试代码的匹配更接近自然语言，可读性更高，更加灵活。

- junit:junit
- org.mockito:mockito-core
- org.robolectric:robolectric
- com.android.support.test.espresso
- com.android.support.test:testing-support-lib
- org.powermock:powermock

#16.代码质量

Effective Java 读书笔记

重构读书笔记

代码清洁之道读书笔记

#17.一些时兴的东西

## RxJava

- RxJava 的作用
- 与平时使用的异步操作来比，优缺点
- 内存泄漏的问题
- 操作符
 - [RxJava 1.x ：创建型操作符](http://blog.csdn.net/u011240877/article/details/75282790)
 - [RxJava 1.x ：变换型操作符](http://blog.csdn.net/u011240877/article/details/74993683)
 - [RxJava 1.x ：过滤型操作符](http://blog.csdn.net/u011240877/article/details/75194948)
 - [ RxJava 1.x ：组合型操作符](http://blog.csdn.net/u011240877/article/details/76162500)
- RxJava2 有什么修改
 - https://juejin.im/post/582b2c818ac24700618ff8f5
 - http://www.vogella.com/tutorials/RxJava/article.html#subscribing-in-rxjava
- RxJava 原理分析，比如如何完成的线程切换,参考 http://blog.csdn.net/column/details/13134.html,参考 Android 进阶之光
- RxLifecycle 生命周期管理，内存泄漏？ https://github.com/trello/RxLifecycle
- RxJava 实战中如何使用
 - 网络请求中线程切换（Retrofit + RxJava）
 - [async-injection-in-dagger-2-with-rxjava](https://medium.com/@froger_mcs/async-injection-in-dagger-2-with-rxjava-e7df503343c0)
 - 常见业务如何结合操作符使用
 - [All RxJava - 为Retrofit添加重试](http://www.jianshu.com/p/fca90d0da2b5)
 - RxLifecycle 如何封装
 - 重要的是思想


### Kotlin

#18.翻译的一些文章

职业发展：

- [ 谷歌求职记：我花了八个月准备谷歌面试](http://blog.csdn.net/u011240877/article/details/53706155)
- [ [干货分享] 反省我十年开发犯过的错](http://blog.csdn.net/u011240877/article/details/53125079)

技术相关：

- [使用流动控制器（Flow Controller ）实现 MVVM 协议模型](http://blog.csdn.net/u011240877/article/details/52149860)
- [ Android ANR 产生原因和解决办法](http://blog.csdn.net/u011240877/article/details/45086057)
- [ 【趣读官方文档】1.管家的抉择 （Android进程生命周期）](http://blog.csdn.net/u011240877/article/details/51014300)
- [ 【苦读官方文档】2.Android应用程序基本原理概述](http://blog.csdn.net/u011240877/article/details/51115718)
- [ Activity 启动模式完全理解：standard, singleTop, singleTask 以及 singleInstance](http://blog.csdn.net/u011240877/article/details/49912685)

其他内容：

- [移动应用设计新趋势](http://blog.csdn.net/u011240877/article/details/52149753)

#19.其他 

- [ 正则表达式简介及学习网址、测试网址](http://blog.csdn.net/u011240877/article/details/47165629)
- [@SuppressWarnings 使用及支持的参数](http://blog.csdn.net/u011240877/article/details/72520836)
- [ sql 删除一条记录后其他记录的 id 自动迁移，使 id 连续](http://blog.csdn.net/u011240877/article/details/46389821)
- XML  JSON 的手动解析


>原文地址：http://blog.csdn.net/u011240877

# 总结

不知不觉写了这么多，却发现差的还有很多很多。学无止境，加油！