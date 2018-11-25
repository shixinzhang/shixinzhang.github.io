---
layout: post
title: React 小练习：实现一个 todolist
categories: FrontEnd
description: 通过练习上手 React
keywords: 前端 React
---

通过练习上手 React。

>学习自慕课网视频：[React 16.4 开发简书项目从零基础入门到实战](https://coding.imooc.com/class/229.html)


# 使用 React 编写 TodoList



React 中 ``render()`` 函数中返回的内容，最外层只能有一个标签，不能有多个标签，有点类似安卓中的 ``ScrollView`` 子布局必须都在一个布局里一样。



React 中的 ``Fragment`` 表示一个占位的元素，可以用它作为最外层元素，避免多一个标签。



```

import React, { Component, Fragment } from 'react';



class TodoList extends Component {

  render() {

    return (

     <Fragment>

       <input onChange={this.handleInputChange()}/> 

       <button>确定</button>



       <ul>

        <li>学前端</li>

        <li>学安卓</li>

       </ul>

      </Fragment>

    );

  }

}



export default TodoList;

```



# React 中的事件绑定和响应式思想



感觉和安卓开发中的 ``ListView`` 设计思想类似，数据通过 adapter 和布局绑定起来。



```

import React, { Component, Fragment } from 'react';



class TodoList extends Component {

  constructor(props){

    super(props);

    // this.state 里保存状态数据，是一个对象，逗号分隔变量

    this.state = {

      inputValue: 'zsx',

      list: []

    }

  }



  render() {

    return (

     <Fragment>

       <input

          // 要使用组件的变量、方法，需要使用花括号表达式{} 包裹起来 

          value={this.state.inputValue}

          // 通过 bind(this) 对作用于进行变更

          onChange={this.handleInputChange.bind(this)}

        /> 

       <button>确定</button>



       <ul>

        <li>学前端</li>

        <li>学安卓</li>

       </ul>

      </Fragment>

    );

  }



  // 保存状态数据，要通过 this.setState 方法，参数也是一个表达式

  handleInputChange(e){

    this.setState({

      inputValue : e.target.value

    })

  }

}



export default TodoList;

```



**状态和数据绑定，通过修改 state 里的数据来改变状态**



> React 中绑定事件的首字母要大写 ``onChange``  ``onClick``


# 实现新增删除功能



> 使用到的方法一定要定义好，不然 React 运行会报错。



**JSX 根据数组数据显示列表：**



```

       <ul>

          {

            this.state.list.map(

              (item, index) => {

                return <li>{item}</li>

              }

            )

          }

       </ul>

```



> this.state.list 是一个数组

> this.state.list.map() 里面是一个箭头函数（好像不是？？），参数是 item 和 index，返回的是要显示的内容

> return 的 JSX 里使用变量别丢了 {}: ``return <li>{item}</li>``



## **添加一个 todo:**



```

  // 点击确定按钮，添加数据

  handleBtnClick(e) {

    this.setState({

      list: [...this.state.list, this.state.inputValue],

      inputValue: ''

    })

  }

```



## **删除一个 todo：**







```

       <ul>

          {

            this.state.list.map(

              (item, index) => {

                // 点击事件里传入 index参数

                return <li onClick={this.handleItemDelete.bind(this, index)}>{item}</li>

              }

            )

          }

       </ul>

```



>展开运算符：``...``，比如把 list 展开：``...this.state.list``



```

  // 点击 item 删除

  handleItemDelete(index) {

    //state 数据是 immutable 的，不允许直接修改

    //避免在修改过程中访问出错



    //需要拷贝一个副本，然后修改副本，再赋值回去



    //直接赋值操作的还是一个内容

    const list = [...this.state.list];



    list.splice(index, 1);



    this.setState({

      list: list

    })

  }

```



# JSX 细节



>label 标签帮助我们聚焦，它可以绑定一个 id，点击 label 就等于点击那个 id 的标签。



- 标签外部，注释需要用花括号包起来： ``{/* 这里是注释 */}``

- React 中，``label`` 标签不能使用 ``for`` 属性，需要使用 ``htmlFor``



```

render() {

    return (

     <Fragment>

        {/*斜杠注释不好使了，得这样才行*/}

        <label htmlFor="insetArea">输入内容</label>

       <input

          id="insetArea"

          // 要使用组件的变量、方法，需要使用花括号表达式{} 包裹起来 

          value={this.state.inputValue}

          // 通过 bind(this) 对作用于进行变更

          onChange={this.handleInputChange.bind(this)}

          />

        

      </Fragment>

    );

```

