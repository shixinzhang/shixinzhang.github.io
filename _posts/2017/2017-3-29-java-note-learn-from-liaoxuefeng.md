---
layout: post
title: 廖雪峰的 JS 教程笔记
categories: JavaScript
description: 廖雪峰的 JS 教程笔记，未完成
keywords: JavaScript, 笔记，廖雪峰
---


[廖雪峰的 JS 教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014344991049250a2c80ec84cb4861bbd1d9b2c0c2850e000) 学习笔记

> 未完成

1.ECMA 制定 JavaScript 标准，称为 ECMAScript。

- ECMAScript 是一种语言标准
- JavaScript 是网景公司对 ECMAScript 的具体实现

一般情况下，ECMAScript == JavaScript。

最新的是 ES6（2015 年 6 月发布），说 JavaScript 的版本，其实说的是它实现了哪个版本的 ECMAScript。

2.JavaScript 严格区分大小写！

3.JavaScript　不区分整数和浮点数，统一用 Number 表示。

4.JS 有两种相等运算符：

- ``==``，两个等号，会自动转换数据 类型，然后再比较
- ``===``，三个等号，不会自动转换类型，如果不一致就返回 false，一致再比较

>唯一的例外是 NaN 这个特殊的家伙，与所有值都不相等，包括自己，也就是说 ``NaN === NaN`` 也返回 ``false``
>可以使用 ``isNaN(NaN)`` 方法比较。

**建议使用三等号比较是否相等**

5.注意浮点数的相等比较：

```
1/3 === (1- 2/3)  //会返回 false
```

因为 ``1/3`` 是无限循环小数，在运算过程中会丢失精度，因此判断无限循环小数时，只能计算它们的绝对值是否小于某个阈值：

```
Math.abs(1/3 - (1 - 2/3)) < 0.0000001;    //true
```

6.几个关键字：

- ``null`` : 空
- ``0``：是一个数值
- ``''``：长度为 0  的字符串
- ``underfine``：未定义

7.JS 的数组中的元素可以是不同类型的：

```
[1, 2, 3.14, 'ZSX', null, true];        //允许
```

8.对象

JS 的对象是由 key-value 组成的无序组合：

```
var person = {
    name: 'shixinzhang',
    age: 24,
    tags: ['android', 'js'],
    city: 'Shanghai',
    zipcode: null
}
```

可以看到，和 JSON 的格式一致！

要获取属性值，直接使用 ``对象变量.属性名`` 即可：

```
person.name;    //shixinzhang
person.city;    //Shanghai
```

9.变量

JS 是动态语言，变量只要声明后，可以被赋值为任意类型：

```
var a = 111;    
a = 'zsx';    //允许
```

对应的静态语言比如 Java，这样就会报错！

10.strict 严格模式

开启严格模式，要求开发者声明变量时必须使用 ``var``，**因为不使用 ``var`` 声明的变量就成了全局变量**，在同一个页面中，如果多个 js 文件中有相同名称的全局变量，可能会导致异常。

使用 ``var`` 声明后的变量就变成了**局部变量**，它被限制在函数体内。

**启用 strict 模式的方法是在 js 文件的第一行声明：**

```
'use strict';
```

11.字符串中的转义字符

如果字符串 （被``''`` 或者 ``""`` 包围的字符）中还会包括单引号、双引号或者其他特殊符号，就需要使用转义字符转义： 

```
'I\'m \" BIG BOY \" !';
```

ASCII 字符可以以十六进制表示，比如： ``\x41`` 等同于 ``A``

12.多行字符串

在有大量的换行字符中，频繁使用 ``\n`` 比较费事，ES6 标准新增了使用反引号 `` 包括字符串的形式，在其中包裹的字符串将保持换行格式：

```
alert(`多行
字符串
测试`);
```

<div align="center">
	<image src="http://oqg4nua5z.bkt.clouddn.com/image/js-liaoxuefengjs-lxf-0.png"/>
</div>

13.模板字符串（占位符）

ES6 新增的模板字符串（使用 ``${变量名}`` 表示）：

```
var name = 'shixin';
alert('ni hao, ${name}' );

```

##2017年4月10日星期一

1.JS 中的字符串和 Java 中的一样，是不可变的，调用字符串的一些方法不会改变本身，而是会创建新的字符串返回。

2.再重复一次：**JS 中的数组可以同时包括各种类型的数据！**

修改数组时，数组的 length 也会跟随者动态改变，反之亦然：

3.数组的添加和删除：

```
//push 是往尾部添加元素，返回数组长度
//	alert(arr.push("A", 'B'));
//unshift 是往头部添加元素
alert(arr.unshift("A", 'B'));
document.write("<br />")
//移除并返回尾部元素
//	alert(arr.pop());
//移除并返回头部元素
alert(arr.shift())
```

**数组的 ``splice()`` 可以用来切割、替换元素、添加元素甚至复制一个数组**:

```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

4.JS 的对象

```
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
```

- **最后一个属性不用加逗号**，如果加了，在低版本的浏览器上可能会报错
- 命名最好规范，不要使用特殊字符，那样访问就可以直接使用 object.XX 

<div align="center">
	<image src="http://oqg4nua5z.bkt.clouddn.com/image/js-liaoxuefengjs-lxf-1.png"/>
</div>


>实际上，JS 对象的属性名都是字符串类型，而值可以使各种类型。

5.返回函数的函数

函数也可以返回一个函数：

<div align="center">
	<image src="http://oqg4nua5z.bkt.clouddn.com/image/js-liaoxuefengjs-lxf-2.png"/>
</div>

上图中，``a()()`` 就相当于先调用 a 得到返回值函数然后继续调用返回函数。

**a()() 等于直接调用函数返回的函数！**

和他相似的是 **函数自调用**：

<div align="center">
	<image src="http://oqg4nua5z.bkt.clouddn.com/image/js-liaoxuefengjs-lxf-3.png"/>
</div>

函数体最后直接加个括号，表示自己就调用了自己。

6.重写函数

在函数内部创建一个函数，然后把它赋值给当前函数的对象，就相当于重写函数：

<div align="center">
	<image src="http://oqg4nua5z.bkt.clouddn.com/image/js-liaoxuefengjs-lxf-4.png"/>
</div>
