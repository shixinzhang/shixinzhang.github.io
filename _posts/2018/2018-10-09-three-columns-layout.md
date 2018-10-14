---
layout: post
title: CSS 学习之 三栏布局（圣杯布局、双飞翼布局）
categories: FrontEnd
description: 学习笔记
keywords: 前端 笔记 布局 圣杯布局
---

圣杯布局、双飞翼布局其实就是三栏布局，是比较常见的一种布局，据说是面试常客。

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539515891169&di=7b3f305eba81a0de9ec0720412c7dcf8&imgtype=0&src=http%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F9397803-e964c6f980b5da16.png)

##  圣杯布局和双飞翼布局

- [CSS布局之--淘宝双飞翼布](http://www.cnblogs.com/langzs/archive/2013/01/27/taobaoshuangfeiyi.html)
- [圣杯布局](https://alistapart.com/article/holygrail)
- [双飞翼布局](http://www.imooc.com/wenda/detail/254035)

实现普通的三列布局比较简单，只要使用下面的布局，然后设置左浮动即可：

```
<div class="sub">子列</div>
<div class="main">主列</div>
<div class="extra">附加列</div>
```

这种结构的布局，浏览器在加载时的顺序：【子】、【主】、【附加】，如果要实现“重要的内容先加载”，就需要换一种布局和 CSS 实现。

![](https://upload-images.jianshu.io/upload_images/1747023-4b4ebc49181a2e4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

**圣杯/双飞翼布局主要解决俩问题：**

1. 三列布局，中间宽度自适应，两边定宽
2. 中间栏要在浏览器中优先展示渲染

首先实现三列布局（同时中间的布局先加载）比较简单，下面的布局，加上【浮动和负边距】，可以实现布局。

```
<div id="bd">
		<div class="main"></div>
		<div class="sub"></div>
		<div class="extra"></div>
	</div>
```

但存在的问题是，中间的内容会被左右覆盖一部分。

要调整中间的内容到合适位置，有两种实现思路：

1. 给父布局设置两侧 padding，然后给左右布局使用相对定位
2. 在中间内容中添加一层 div，给这个 div 设置外边距，这样就能保证实际的内容不会被覆盖
3. 直接给中间布局设置 ``box-sizing: border-box;`` 和 ``padding:0 190px 0 240px;``

思路 1 的实现代码：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>双飞翼布局</title>
	<style type="text/css">
		* {
			box-sizing: border-box;
		}
		#bd {
			padding: 0 230px 0 190px;
			min-width: 610px;
		}
		.main {
			float: left;
			width: 100%;
			height: 500px;
			background: red;
		}
		.sub {
			float: left;
			width: 190px;
			height: 500px;
			/*浮动情况下，负边界值会导致DIV上移，而使用-100%可以确实它移动到最左侧*/
			margin-left: -100%;
			background: yellow;
			position: relative;
			left: -190px;
		}
		.extra {
			float: left;
			width: 230px;
			height: 500px;
			/*负左边界一定要大于或等于该DIV的宽度，才能靠到上一行去*/
			margin-left: -230px;
			background: blue;
			position: relative;
			right: -230px;
		}

	</style>
</head>
<body>
<div id="page">
	<div id="hd"></div>
	<div id="bd">
		<div class="main"></div>
		<div class="sub"></div>
		<div class="extra"></div>
	</div>
	<div id="ft"></div>
</div>
</body>
</html>
```

思路 2 的实现代码：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>双飞翼布局</title>
	<style type="text/css">
		* {
			box-sizing: border-box;
		}
		#bd {
			/*padding: 0 230px 0 190px;*/
			min-width: 610px;
		}
		.main {
			float: left;
			width: 100%;
			height: 500px;
		}
		/*包裹的一层，设置左右外边距*/
		.main-wrap {
			margin-left: 200px;
			margin-right: 240px;
			background: grey;
			height: 500px;
		}
		.sub {
			float: left;
			width: 190px;
			height: 500px;
			/*浮动情况下，负边界值会导致DIV上移，而使用-100%可以确实它移动到最左侧*/
			margin-left: -100%;
			background: yellow;
			/*position: relative;
			left: -190px;*/
		}
		.extra {
			float: left;
			width: 230px;
			height: 500px;
			/*负左边界一定要大于或等于该DIV的宽度，才能靠到上一行去*/
			margin-left: -230px;
			background: blue;
			/*position: relative;
			right: -230px;*/
		}

	</style>
</head>
<body>
<div id="page">
	<div id="hd"></div>
	<div id="bd">
		<div class="main">
			<div class="main-wrap">
				我的主内容
			</div>
		</div>
		<div class="sub"></div>
		<div class="extra"></div>
	</div>
	<div id="ft"></div>
</div>
</body>
</html>
```

思路 3 实现：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>双飞翼布局</title>
	<style type="text/css">
		* {
			/*box-sizing: border-box;*/
		}
		#bd {
			/*padding: 0 230px 0 190px;*/
			min-width: 610px;
		}
		.main {
			float: left;
			width: 100%;
			height: 500px;
			box-sizing: border-box;
			padding:0 190px 0 240px;
			background: red;
		}
		.sub {
			float: left;
			width: 190px;
			height: 500px;
			/*浮动情况下，负边界值会导致DIV上移，而使用-100%可以确实它移动到最左侧*/
			margin-left: -100%;
			background: yellow;
			/*position: relative;
			left: -190px;*/
		}
		.extra {
			float: left;
			width: 230px;
			height: 500px;
			/*负左边界一定要大于或等于该DIV的宽度，才能靠到上一行去*/
			margin-left: -230px;
			background: blue;
			/*position: relative;
			right: -230px;*/
		}

	</style>
</head>
<body>
<div id="page">
	<div id="hd"></div>
	<div id="bd">
		<div class="main">我的主内容</div>
		<div class="sub"></div>
		<div class="extra"></div>
	</div>
	<div id="ft"></div>
</div>
</body>
</html>
```

**可以看到，第一种思路纯使用 CSS 实现，CSS 略微复杂了一些；第二种思路 CSS 比较简单，但需要多增加一个标签。目前淘宝好像使用的是第二种思路。**

第三种是网友提出的，也很简单，缺点是左右布局底层其实是有中间布局的，只不过看不到。

> 如果把三栏布局比作一只大鸟，可以把main看成是鸟的身体，sub和extra则是鸟的翅膀。
> 
> 这个布局的实现思路是，先把最重要的身体部分放好，然后再将翅膀移动到适当的地方。因此请容许我给这个布局实现取名为**双飞翼布局（Flying Swing Layout）**.

上面的代码中，我们可以看到：需要设置其**左边距为负的中间盒子的宽度**，这样左盒子才可以往最左边移动。

浮动与负边距有什么不可告人的秘密呢？

答案是：**浮动情况下，（超过本身宽度的）负边界值会导致 DIV 上移，而使用 -100% 可以确使它移动到最左侧。**

>当你缩放页面的时候，宽度不能小于 700px，为了安全起见，最好还是给 body 加一个最小宽度!

