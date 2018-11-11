---
layout: post
title: React 初探
categories: FrontEnd
description: 前端学习之 React
keywords: 前端 React
---

初步了解 React。

![](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9Ii0xMS41IC0xMC4yMzE3NCAyMyAyMC40NjM0OCI+CiAgPHRpdGxlPlJlYWN0IExvZ288L3RpdGxlPgogIDxjaXJjbGUgY3g9IjAiIGN5PSIwIiByPSIyLjA1IiBmaWxsPSIjNjFkYWZiIi8+CiAgPGcgc3Ryb2tlPSIjNjFkYWZiIiBzdHJva2Utd2lkdGg9IjEiIGZpbGw9Im5vbmUiPgogICAgPGVsbGlwc2Ugcng9IjExIiByeT0iNC4yIi8+CiAgICA8ZWxsaXBzZSByeD0iMTEiIHJ5PSI0LjIiIHRyYW5zZm9ybT0icm90YXRlKDYwKSIvPgogICAgPGVsbGlwc2Ugcng9IjExIiByeT0iNC4yIiB0cmFuc2Zvcm09InJvdGF0ZSgxMjApIi8+CiAgPC9nPgo8L3N2Zz4K)

https://reactjs.org/docs/create-a-new-react-app.html#create-react-app

>学习自慕课网视频：[React 16.4 开发简书项目从零基础入门到实战](https://coding.imooc.com/class/229.html)

## [2.5 React 中最基础的 JSX 语法](https://coding.imooc.com/lesson/229.html#mid=15490)



在 js 中写的标签（ HTML 标签及自己定义的组件），就是 JSX 语法。



>自定义标签/组件，必须以大写开头。



小写开头的标签内容，一般是原生的；大写开头的，一般是自定义的。



## [2.4 React 中的组件](https://coding.imooc.com/lesson/229.html#mid=15489)



什么是组件：



```

// 继承 React.Component 的就是组件

class App extends React.Component {

  // render 方法返回 组件要显示的内容

  render() {

    return (

      <div className="App">

        Hello React

      </div>

    );

  }

}

```

[解构赋值](https://www.cnblogs.com/pixcai/p/5597109.html)：获得对象的同名属性



```

import { Component } from 'react';

// 等价于

import React from 'react';

const Component = React.Component;



// 然后就可以直接继承 Component

class App extends Component {

  render() {

    return (

      <div className="App">

        Hello React

      </div>

    );

  }

}

```



``ReactDOM`` 的作用：将组件挂载到某个 dom 节点上



```

ReactDOM.render(<App />, document.getElementById('root'));

```



><App /> 是 JSX 语法，需要引入 react 才能编译



## [2.3 工程目录简介](https://coding.imooc.com/lesson/229.html#mid=15488)

核心文件就这么几个：``index.html``, ``index.js``, ``App.js``:




入口 js 文件是 ``index.js``



>React 是 "all in js"，样式也在 js 文件中。



##  [2.2 creat-react-app 脚手架的环境搭建](https://coding.imooc.com/lesson/229.html#mid=15487)



>脚手架就是帮助我们搭建项目结构的工具



刚开始，安装 ``create-react-app`` 超时、使用 ``create-react-app`` 初始化一个项目也超时：



```

error An unexpected error occurred: "http://10.16.45.29:4873/react: ETIMEDOUT"
```



安装时超时的解决办法是使用 ``cnpm``；

初始化项目时超时的解决办法是，修改注册源：



```

npm config set registry https://registry.npm.taobao.org 

-- 配置后可通过下面方式来验证是否成功 

npm config get registry 

-- 或npm info express

```

>https://blog.csdn.net/eagyne/article/details/53780653