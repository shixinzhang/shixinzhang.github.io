---
layout: post
title: React 笔记：生命周期函数及使用场景
categories: FrontEnd
description: 通过练习上手 React
keywords: 前端 React
---

了解 React 生命周期及使用场景。

>学习自慕课网视频：[React 16.4 开发简书项目从零基础入门到实战](https://coding.imooc.com/class/229.html)

# React 的生命周期函数



生命周期函数，即在某一时刻，组件会自动调用执行的函数，比如 ``render()``。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181203001445887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTEyNDA4Nzc=,size_16,color_FFFFFF,t_70)


每一个组件都有这些生命周期函数：



1. 初始化 

2. 挂载 

3. 更新

4. 移除



## 初始化



``constructor()`` 里，设置　props states。





## 挂载



组件第一次显示在页面，称作“挂载”。





``componentWillMount`` 在组件即将被**挂载到页面之前**自动执行;

``componentDidMount`` 在组件即被**挂载到页面之后**自动执行。





**第一次渲染时：``componentWillMount`` -> ``render`` -> ``componentDidMount``**





## 更新



- props 变化

- states 变化





### props states 变化都有的





``shouldComponentUpdate`` 在页面刷新（组件数据变更）之前执行，返回值决定当前组件是否需要更新。

如果返回 true 表示可以更新；false 表示不可以更新。







>可以在这个函数里做拦截，拦截不必要的更新。





``componentWillUpdate`` 组件被更新之前自动执行。





如果 ``shouldComponentUpdate`` 返回 true，``componentWillUpdate`` 会被执行；否则不会。





**组件更新时：``shouldComponentUpdate`` -> ``componentWillUpdate`` -> ``render`` -> ``componentDidUpdate``**





### props 变化才有的





当一个组件要从父组件接收参数，只要父组件的 render 函数被**重新**执行了，子组件的 ``componentWillReceiveProps`` 函数会被执行。





换句话说，子组件第一次出现在父组件中，不会执行``componentWillReceiveProps``；已经存在，父组件重新 render 时，才会执行。





## 移除





组件被从父组件里移除时，会执行 ``componentWillUnmount``。





# 生命周期函数的使用场景





## 避免不必要的渲染





为什么组件里必须实现 render() 函数？



因为 React 的 Component 类里已经默认实现了其他生命周期函数，唯独没有实现这个 render 函数。





父组件执行 render() 时子组件的 render　也会被执行。这在子组件不需要更新时，是一种性能浪费。



我们可以通过在子组件的　``shouldComponentUpdate``　里返回　false 实现不更新。





不过也不能无脑 false，可以判断下数据有没有变化再决定。



比如当前组件里的一个 content 属性非常重要：





```
shouldComponentUpdate(nextProps, nextState){

 if(nextProps.content != this.props.content) {

  return true;

 }else {

  return false;

 }

}

```



## 避免重复执行





只需要执行一次的，放到 constructor 函数里，比如方法作用域的绑定。



```
constructor(props) {

 super(props);

 this.handleClick = this.handleClick.bind(this);

}

```



## 何时获取数据





不能在 render 函数里，因为这个函数会被执行多次。





可以放在 ``componentDidMount`` 里，它只在组件被挂载在页面上时执行一次。





推荐的 ajax 网络请求扩展工具：axios



```
yarn add axios

```



```
import axios from 'axios'



componentDidMount() {

 axios.get('/api/tdolist')

 .then(() => { alert('succ') })

 .catch(() => { alert('error') })

}

```



![在这里插入图片描述](https://img-blog.csdnimg.cn/20181203001541470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTEyNDA4Nzc=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181203001615736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTEyNDA4Nzc=,size_16,color_FFFFFF,t_70)
