---
layout: post
title: Android 面试相关内容汇总
categories: GrowNote Android
description: 记录面试相关的内容
keywords: 安卓 面试 资料
---

## 工作五年前辈分享各阶段能力

![](http://oqg4nua5z.bkt.clouddn.com/%E8%A7%84%E5%88%920.jpeg)

## 面试考察点

- 项目经验（项目复杂度、项目贡献） 
- 架构能力 
- 模块设计能力 
- 解决问题能力 
- Android 基础 /Java 基础 
- 沟通能力 
- 自我提升能力 
- 推动能力 

**对客户端开发来说，计算机网络的考察会比较多，TCP 和 UDP 的区别、TCP 的拥塞控制、TCP 的握手与挥手流程、HTTP 与 HTTPS 的差别等等。基本面的所有公司都问到这块了。**

做了什么优化相关的工作(能体现出你的优秀品质的）。

## 知识点

用MultiDex解决何事？其根本原因在于？Dex如何优化？主Dex放哪些东西？主Dex和其他Dex调用、关联？Odex优化点在于啥？Dalvik和Art虚拟机区别？多渠道打包如何实现（Flavor、Dimension应用）？从母包生出渠道包实现方法？渠道标识替换原理？

### 并发

### 网络

https的原理就是，用对称加密方式加密传输数据，用非对称加密方式对 对称加密的密钥进行加密，保证了该密钥的安全也就保证了由该密要加密后的数据的安全。

### 性能优化

## 资料链接

- 一个不错的基础知识复习资料，包括操作系统、网络原理啥的：https://github.com/iwannabetop/Interview-Notebook

## 小题目

先问你内存怎么优化，你回答完bitmap io 线程池这些，又继续追问你bitmap怎么优化，线程池怎么优化

## 2018 新鲜面试题

### 淘宝高级安卓

移动应用 APM、即时通讯、动态部署、架构设计、组件化、Gradle 插件、跨平台

网络优化，包括域名收敛、HTTPDNS

### 哔哩哔哩

1. Fragment 生命周期
2. Fragment comit 干了什么事情
3. Fragment 状态保存
4. Synchronized 和 Lock 区别
5. Handler 源码
6. 所有 View ondraw 里面的Canvas是否是同一个对象？我回答是，每个 view canvas 的绘制是怎么做到互补干扰的。应该是通过saveLayout()和 restore()，请讲一下这两个方法干了什么事情，什么时候用
7. 类加载器相关
8. 一个小动画的实现思路

### 平安好房

电面很简单，跳过

1. Handler生命周期
2. 算法题，链表中出现了链环怎么跳出
3. Retrofit 怎样将 okhttp 的 callback 转换成Observe
4. 自己实现图片加载框架，如果服务器 url 的图片源切换了，在本地已经有缓存的情况下，怎样更新图片（必须做三级缓存）
5. 谈谈 picasso 和 glide

### FlipBoard

1. 事件传递机制
2. 两种动画，自定义属性动画
3. OKHTTP原理
4. LruCache原理，自己实现的话怎么设计
5. 自定义View，View的绘制流程
6. 内存优化，怎么检测内存泄漏，leakcanary原理和源码分析
7. 讲讲并发包中的内容，用Atomic原子类的优点，ReentrantLock的内部原理
8. kotlin和Java区别，run,with内部实现
9. kotlin中Constructor和init方法哪个先执行
10. 讲讲kotlin携程
11. 100个数中输出最大的五个，保证只循环一遍
12. 讲讲常用的设计模式
13. 红黑树的特性

### 猎豹

1. 属性动画和传统动画的实现原理
2. synchronized和Reentrantlocak的使用方法以及原理分析
3. view的绘制流程
4. 内存泄漏、内存优化、性能优化
5. 几种布局的性能分析
6. RxJava中几种线程切换为什么IO线程是单独分出来的
7. concurrentHashMap的实现原理
8. 进程间通信的几种方式、为什么socket传输速度最快
9. retrofit、okhttp的源码分析

### vite

1. volatile关键字有什么作用，为什么能实现有序性，为什么处理器要进行指令重排序
2. activity生命周期谁管理的
3. 事件传递
4. Java的内存模型
5. Java回收机制
6. 常用设计模式，设计模式的作用
7. 红黑树的一些特性
8. 什么是数字签名
9. Https的原理，证书是干什么的
10. 常见的加密算法，非对称加密和对称加密的区别
11. 讲讲什么是哈希算法
12. 链表和数组的区别，分别怎么实现插入和删除的
13. 比特币钱包的加密算法、比特币原理，区块链的运用场景
14. Java内存回收机制，讲讲引用计数法和可达分析算法，可以作为GC Roots的对象由哪些

 
### 爱回收


* 自定义view
* 有没有自定义过gradle插件
* mvvm mvp 
* listview recyclerview 异同
* 简述java常用的数据结构
* hashmap sparsearray区别 对比
* 项目难点，如何解决
* 某个文件夹下全部的文件名 
* 常用框架源码解析
* jetpack库



### 喜马拉雅


* 项目难点，如何解决
* 常用框架源码解析
* mvvm mvp
* 多线程同步，乐观锁 悲观锁
* 某个文件夹下全部的文件名 
* 同步锁在方法 静态方法  类 对象 区别
* view绘制流程 从xml->java
* 自定义view  onmeasure->onlayout->ondraw 具体
* 一个int[]包含多个int值，o(1)剔除
* 手写伪代码框架代码
* currxxxxhanshmap用过吗（答：没）
* hashmap 源码
* 树查询。。。

### 今日头条

> 去头条之前  先补补算法 知识     基础一定要扎实

- for和foreach(会报异常) remove 集合
- 2T文件内容只有200m如何排序(归并)
- 文件上传分片上传设计
- 屏蔽别人SDK Activity返回事件
- RecyclerView深度优化（布局释放内存释放等）
- Https 缓存描述
- Okhttp的拦截器种类

##面试总结

![](http://oqg4nua5z.bkt.clouddn.com/%E9%9D%A2%E8%AF%95%E9%A2%981.jpeg)


![](http://oqg4nua5z.bkt.clouddn.com/%E9%9D%A2%E8%AF%95%E9%A2%982.jpeg)

![](http://oqg4nua5z.bkt.clouddn.com/%E9%9D%A2%E8%AF%95%E9%A2%983.jpeg)

![](http://oqg4nua5z.bkt.clouddn.com/%E9%9D%A2%E8%AF%95%E9%A2%984.jpeg)


![](http://oqg4nua5z.bkt.clouddn.com/blog/%E9%9D%A2%E8%AF%95.jpeg)

![](http://ww1.sinaimg.cn/large/b1aad299gy1ft4ob60luzj20ku3k44p7.jpg)

