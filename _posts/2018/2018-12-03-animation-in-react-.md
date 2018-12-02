---
layout: post
title: React 笔记：动画
categories: FrontEnd
description: 通过练习上手 React
keywords: 前端 React
---

了解 React 中的动画。

>学习自慕课网视频：[React 16.4 开发简书项目从零基础入门到实战](https://coding.imooc.com/class/229.html)


# 4.10 使用 Charles mock 数据



创建一个文件：``touch todolist.json``





在里面写上简单的数据。





Charles -> Tools -> Map local






# 4.11 React 中实现 CSS 过渡动画





根据数据修改标签的 class





```
render() {



 return (

  <div className={this.stats.show ? 'show' : 'hide'}></div>

 )

}

```



> 注意，这里的类选择器是 ``className``





和 CSS3 的过渡动画一样，给 style 样式里增加 ``transition`` 属性即可：





```
.show {

 opacity: 1;

 transition: all 1s ease-in;

}



.hide {

 opacity: 0;

 transition: all 1s ease-in;

}

``` 



# 4.12 React 中使用 CSS 动画效果





通过 ``@keyframes`` 定义动画，然后在选择器的 ``animation`` 属性里使用。





```
@keyframes hide-item {

 0% {

  opacity: 1;

  color: red;

 }



 50% {

  opacity: 0.5;

  color: green;

 }

	

 100% {

  opacity: 0;

  color: blue;

 }

}

```



里面的百分比表示执行到百分比要设置的值。





选择器里使用：



```
.hide {

 animation: hide-item 2s ease-in forwards;

}

```



最后的 ``forwards`` 表示动画结束后保持最后的状态。






# 4.13、14 使用 react-transition-group 实现动画





> Github 里搜这个项目，3000 star





```
import { CSSTrasition } from 'react-transition-group';

```



然后在 render 的 JSX　里使用。





```
<CSSTransition

	

>



 <div>hello</div>

</CSSTransition>



```



CSSTransition 会自动往我们的组件的 css 里挂载样式，在动画的不同阶段，会挂载不同的选择器样式。





## 多个元素使用 CSSTransition 动画



>https://blog.csdn.net/liangklfang/article/details/72865590







