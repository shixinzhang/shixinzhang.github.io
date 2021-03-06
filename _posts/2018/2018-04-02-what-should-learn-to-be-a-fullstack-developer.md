---
layout: post
title: 前端后台学习内容记录
categories: Plan
description: 记录前端后台需要学习的内容
keywords: 思考，反馈
---

互联网创业公司，早期一般都是同事、同学或者朋友联合创业，早期的CTO可能就是一个技术工程师来担任，对产品与技术能力要求也并不是特别强，能够将基本产品设计开发出来就ok了。

但是A轮融资之后，用户量、交易量、并发量等等面临快速增长，对技术架构、后台性能、用户体验、安全性、产品矩阵、开发效率、人员管理等都提出了更高的挑战，这时候早期的CTO短板就出现了。很多创业公司的CTO不能因应快速成长，个人技能迭代跟不上公司发展需要，就不得不被重构（换人～），例如滴滴出行，拉勾网等很多公司在A后就换CTO了！

# 前端学习内容记录

- [50道CSS基础面试题（附答案）](https://segmentfault.com/a/1190000013325778)
- [精心收集的48个JavaScript代码片段，仅需30秒就可理解](https://mp.weixin.qq.com/s?__biz=MzIzNTU2ODM4Mw==&mid=2247486149&idx=1&sn=83435cd52d365f1f313a6a6980850d5f&chksm=e8e46755df93ee4344800e1c27410ea9c76b8b30401352cf87474de15d1ca2a5f1652ae4e413&mpshare=1&scene=1&srcid=0322ofNQZOLkedtExx4Tztfi#rd)

蚂蚁金服职位要求

1. 对软件开发有浓厚兴趣，熟悉J2EE体系；
2. 熟练掌握移动端H5开发、熟悉主流移动浏览器的技术特点；
3. 熟练运用JavaScript、HTML5、CSS3等；熟悉移动端Web动效相关高级特性, 如canvas, CSS3动画效果等；
4. 熟悉模块化、前端编译和构建工具，熟练运用主流的移动端JS库和开发框架，并深入理解其设计原理，例如：ReactJS、Zepto、AntD等；
5. 能提供完善的WebApp和混合App（JS方向）技术方案，有服务端（node/java或其他语言）或native移动应用开发经验更佳；

知识点：

- JS 特殊概念：闭包，作用域，原型，异步，Promise，递归。不是说记住这些概念，而是切实理解，并且可以熟练运用。面试官不会问你闭包是什么（如果这样问了，那这个面试官的面试能力值得怀疑），而是可能会给你一个场景题目，让你给出答案。
- 正则，至少可以用简单的正则语法。
- 网络，缓存相关，http method，http status code，cookie session 相关，跨域相关，最经典的一个题目：浏览器输入 url 之后发生了什么事情。
- 常见框架（React，Vue）的使用，分层，周边技术栈，组件通信，组件设计，webpack等。
- 开发调试相关，网络调试工具，H5 调试方法，静态资源托管，git 代码管理，mock 方法等等。
- ES6 ES7 新语法类

知识点2：

- 模块化(amd/cmd/umd/ES6 module)
- 跨域多种方式，如jsonp
- js
 - javascript中的this指向问题
 - js异步加载
 - js创建对象的几种方式
 - js继承的几种方式与优缺点
 - js捕获与冒泡
- SEO
- promise与generator
- 服务器推
- jQuery相关
- IE内存泄露
- CORS
- AJAX的几种状态，ajax与fetch，hijax
- iframe与onload阻塞主页面
- 前端安全与CSRF，XSS，SQL注入，DDOS
- drag和drop实现拖拽
- cookie/session/本地存储
- 雅虎网站优化的军规
- css与js的阻塞加载
- chrome/IE浏览器事件兼容
- css相关
 - 垂直水平居中
 - 盒模型
 - 浮动与定位
 - css性能
 - css3的新特性，如flex布局等
- 排版引擎与js引擎
- GPU加速与动画性能
- DOM1,DOM2,DOM3规范
- h标签与title标签
- em与百分比等
- 浏览器缓存与应用缓存
- div与table布局
- web标准
- css的hack技术
- png/jpg/webp图片格式
- canvas与svg
- 响应式布局
- link与import区别
- 三栏自适应
- b和strong,i和em区别
- 减少页面回流
- BFC
- 硬件加速与动画优化
- 前端自动化相关
- webpack相关
 - webpack-dev-server相关
 - 单页面打包工具+多页面打包工具
- babel相关
- 其他知识
 - http/1.1与http2
 - http三次握手协议
 - http状态码
 - json与xml
- 前端性能优化
- nodejs/npm相关内容
- 算法
 - 几种排序算法
 - 回文字符
 - 递归(很重要)
 - 其他常见的前端算法

学习网站：

- [github 上收藏的前端学习指南](https://github.com/shixinzhang/fe)
- [DOM 标准](https://dom.spec.whatwg.org/)
- https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX
- [一个获取数据的例子，可以学学](https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/) 
- [生命一号前端学习文章](http://www.cnblogs.com/smyhvae/category/740114.html)
- [声享，很棒的前端 ppt 分享](https://ppt.baomitu.com/u/shi-nian-zong-ji)

个人博客：

- [校友才盐城的博客，前端](http://www.chaiyanchen.top/#/home/blog)
- [百度前端开发](http://yanhaijing.com/)
- [十年前端！](https://imququ.com/post/series.html)
- [安卓自学几个月转行前端，入职京东](http://www.cnblogs.com/smyhvae/p/8776837.html)

首先是出于对前端的热爱，所以才下了坚定的决心；其次，前端开发和Android开发都是属于大前端领域，二者有很多相通的地方，我在掌握前端的过程中，上手相对顺利；再次，在具备其他技术知识的前提下，去做前端开发，或许可以更好地从全局的角度思考问题。

# 后端学习内容记录

蔚来汽车 岗位要求：

- 精通Java开发，2年以上项目开发经验；
- 精通Spring体系，精通Spring微服务／核心代码／风格制作，1年以上SpringMVC或SpringBoot项目开发经验；
- 熟悉Restful API，能够编写Module开发文档；
- 精通HTML、DIV、CSS代码，掌握JavaScript，AJAX应用；
- 熟练使用LINUX（DEBIAN,CENTOS)环境下的构架进行WEB开发；
- 熟练使用MySQL数据库, 有良好的数据库操作和维护经验；
- 熟悉配置Java环境配置

![](https://img-blog.csdn.net/20180805000410355?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTEyNDA4Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](http://oqg4nua5z.bkt.clouddn.com/blog/backend.jpeg)

学习网站：

- [后端源码分析，这人是全栈](https://www.jianshu.com/u/f7daa458b874)
- [SpringMVC+MyBatis](https://blog.csdn.net/eson_15)


# 云计算

不知道哪儿来的一个课程大纲：

一、基础扫盲：                  

1. 数据中心硬件：硬件技术、Server级硬件管理、RAID硬件管理                   
2. 网络基础：网络体系结构、互联设备、局域网、广域网、Router&Switch实战            
3. 虚拟机的使用、制作安装介质。                  
4. 系统安装：SystemV风格、BSD风格Linux发行版安装及对比                     

二、Linux系统：               

1. 系统命令：熟练操作、查询、字符管理命令、编辑器               
2. 磁盘管理：分区、格式化、iSCSI、Ceph、GFS、LVM…等集群、冗余等技术           
3. 软件管理：软件管理、本地YUM源部署(基于ISO、shell脚本、手动建立等)      
4. 用户管理：用户管理（分散式管理、集中式管理、增、删、改、查、PAM控制等)     
5. 进程管理：进程管理与控制并结合IDC、运维常用的80多项巡检脚本监控系统资源  
6. Shell编程：变量、语句格式、语法格式、Linux系统脚本分析与学习、常用巡检脚本的学习与编写                     
7. 日志管理：查询、收集、集中管理、集群实现与安全               
8. 启动流程：GRUB2管理、维护、编写与Linux系统Troubleshooting处理与思路       

二、Linux网络服务：               

1. 网络管理：网卡配置及7种工作模式、Linux下的Router、VLAN实现、SSH及3大组建使用与管理等               
2. 基础服务：文件服务、DNS、DDNS、DHCP、Apache与集群、Nginx与集群、tomcat等                     
3. 存储服务：分布式存储、远程存储、本地及远程灾备、存储集群等               
4. 安全服务：Nagios、Zabbix、iptables、Firewalld等                
5. 数据库：MariaDB数据库的安装、建立、用户管理、数据的增删改查、集群部署与管理等
6. 中间件：各种分布式高速缓存服务及消息队列(Memcache、RabbitMQ等)                 
7. 集群管理：各种网站及缓存服务器、数据库的高可用、负载均衡                  

三、自动化运维：                     

1. ansible的配置、语法等                 
2. ansible的结构、模块详解                    
3. ansible功能及应用                 
4. GitHub的介绍、安装、及使用。                  

四、虚拟化：

1. 虚拟化详解、安装、透传、热迁移               
2. 基于Web的集中管理                    
3. 命令行管理        

五、云平台       

1. Openstack详解、实验环境、架构拓扑               
2. 图形界面的安装配置               
3. 建立秘钥认证                  
4. 镜像服务的创建、运行、管理               
5. 计算服务的介绍与配置                  
6. 网络服务的管理、负载均衡、高可用集群                  
7. 云平台对接块存储、对象存储               
8. 实例的定制、迁移、热迁移                  
9. 卷管理的快照转让、优化、Heat编排                 
9. Openstack自动化运维、部署、重要数据的计量。                  


六、容器及容器云集群                     

1. 容器详解、安装、命令使用、端口映射               
2. 网络配置、链接容器、构建容器库               
3. 容器编排                  
4. 容器云架构及原理                  
5. 核心组件、单机部署、集群部署