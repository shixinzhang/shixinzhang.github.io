---
layout: post
title: 前端开发时踩的坑
categories: FrontEnd
description: 随笔
keywords: 前端 笔记
---

记录学习前端开发时踩的坑，没事常回来看看！

2018.10.14

### 1.css calc 的坑

坑爹啊！**calc() 函数用于动态计算长度值，需要注意的是，运算符前后 都需要保留一个空格，例如：``width: calc(100% - 10px)；`**

1. calc 括号中的运算符前后都要有一个空格！
2. calc 括号中的运算符前后都要有一个空格！
3. calc 括号中的运算符前后都要有一个空格！

重要的事说三遍，太坑了！

### 2.组合选择器中，一个逗号的差别

使用个简单的组合选择器总不生效，结果发现不同 class 间漏了逗号，太菜了！

**有没有逗号区别还是很大的。**

有逗号，表示给两个 class 设置相同的属性：

```
		.left, .right {
			width: 200px;
			height: 500px;
		}
```

没有逗号，表示给 class left 中的 class right 设置样式：

```
		.left .right {
			width: 200px;
			height: 500px;
		}
```

## 3.伪类选择器 ``nth-of-type`` 前不能有空格

选择第几个同类元素 的伪类：nth-of-type，前面不能有空格，否则无效：

```
/*第一行*/
		p:nth-of-type(1){
			font-size: 20px;
		}
```