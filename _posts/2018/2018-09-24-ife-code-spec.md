---
layout: post
title: 百度前端代码规范学习
categories: FrontEnd
description: 学习笔记
keywords: 前端 笔记
---

学习目标：了解一些通用的代码风格及规范。



[百度前端代码规范](https://github.com/ecomfe/spec)

# 代码规范

https://github.com/ecomfe/spec

### HTML

- id、class 命名，在避免冲突并描述清楚的前提下尽可能短
- 同一页面，应避免使用相同的 name 与 id
- 对于无需自闭合的标签，不允许自闭合（常见无需自闭合标签有 input、br、img、hr 等） 【不是很理解】
- 在 CSS 可以实现相同需求的情况下不得使用表格进行布局 
- 标签的使用应尽量简洁，减少不必要的标签
- 属性值必须用双引号包围 ``<script src="esl.js"></script>`` 而不是单引号
- 自定义属性建议以 xxx- 为前缀，推荐使用 data- ``<ol data-ui-type="Select"></ol>``
- 启用 IE Edge 模式 ``<meta http-equiv="X-UA-Compatible" content="IE=Edge">`` 【这是啥】
- 在 html 标签上设置正确的 lang 属性 ``<html lang="zh-CN">``，为翻译工具提供信息 【加了这个 lang 中文字体会改变，赞】
- 若页面欲对移动设备友好，需指定页面的 ``viewport``
- 为重要图片添加 alt 属性（可以提高图片加载失败时的用户体验）
- 只在必要的时候开启音视频的自动播放

```
<input name="foo">
<div id="foo"></div>
<script>
// IE6 将显示 INPUT
alert(document.getElementById('foo').tagName);
</script>
```

```
<!-- good -->
<img class="avatar" src="image.png">

<!-- bad -->
<span class="avatar">
    <img src="image.png">
</span>
```

### CSS

[css-style-guide](https://github.com/ecomfe/spec/blob/ac7287e54aeab7ec957a939674dd40f3510edea6/css-style-guide.md)

- 引入 CSS 时必须指明 rel="stylesheet" ``<link rel="stylesheet" href="page.css">``
- 引入 CSS 和 JavaScript 时无须指明 type 属性（text/css 和 text/javascript 是 type 的默认值）
- 展现定义放置于外部 CSS 中，行为定义放置于外部 JavaScript 中（结构-样式-行为拆分，对维护有好处）
- **在 head 中引入页面需要的所有 CSS 资源**，（在页面渲染的过程中，新的CSS可能导致元素的样式重新计算和绘制，页面闪烁）
- 使用 4 个空格做为一个缩进层级，不允许使用 2 个空格 或 tab 字符 【违背我的习惯】
- 选择器 与 { 之间必须包含空格
- 属性名 与之后的 : 之间不允许包含空格， : 与 属性值 之间必须包含空格
- 对于超长的样式，每行不得超过 120 个字符
- 在样式值的 空格 处或 , 后换行，建议按逻辑分组
- 当一个 rule 包含多个 selector 时，每个选择器声明必须独占一行 【长见识了】
- 在可以使用缩写的情况下，尽量使用属性缩写
- 使用 ``border / margin / padding`` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写
- 尽量不使用 ``!important`` 声明
- 将 ``z-index`` 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。在可控环境下，期望显示在最上层的元素，z-index 指定为 999999
- **文本内容必须用双引号包围**（文本类型的内容可能在选择器、属性值等内容中）
- url() 函数中的路径不加引号
- RGB颜色值必须使用十六进制记号形式 ``#rrggbb``。不允许使用 rgb()
- 颜色值可以缩写时，必须使用缩写形式
- 使用 transition 时应指定 transition-property

```
//选择器 与 { 之间必须包含空格
.selector {
}
```

```
//属性名 与之后的 : 之间不允许包含空格， : 与 属性值 之间必须包含空格
margin: 0;

```

```
//当一个 rule 包含多个 selector 时，每个选择器声明必须独占一行
/* good */
.post,
.page,
.comment {
    line-height: 1.5;
}

/* bad */
.post, .page, .comment {
    line-height: 1.5;
}
```

--------

**同一 rule set 下的属性在书写时，应按功能进行分组，并以 Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果） 的顺序书写，以提高代码的可读性。**

- Formatting Model 相关属性包括：position / top / right / bottom / left / float / display / overflow 等
- Box Model 相关属性包括：border / margin / padding / width / height 等
- Typographic 相关属性包括：font / line-height / text-align / word-wrap 等
- Visual 相关属性包括：background / color / transition / list-style 等

```
.sidebar {
    /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
    position: absolute;
    top: 50px;
    left: 0;
    overflow-x: hidden;

    /* box model: sizes / margins / paddings / borders / ...  */
    width: 200px;
    padding: 5px;
    border: 1px solid #ddd;

    /* typographic: font / aligns / text styles / ... */
    font-size: 14px;
    line-height: 20px;

    /* visual: colors / shadows / gradients / ... */
    background: #f5f5f5;
    color: #333;
    -webkit-transition: color 1s;
       -moz-transition: color 1s;
            transition: color 1s;
}
```

-------------

**当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 clear 或触发 BFC 的方式进行 clearfix。尽量不使用增加空标签的方式。**

触发 BFC 的方式很多，常见的有：

- float 非 none
- position 非 static
- overflow 非 visible

-----------------

当数值为 0 - 1 之间的小数时，省略整数部分的 0：

```
/* good */
panel {
    opacity: .8;
}

/* bad */
panel {
    opacity: 0.8;
}
```

--------

![](http://oqg4nua5z.bkt.clouddn.com/blog/0-font.jpg)

- font-family 按「西文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( serif / sans-serif )。
- 需要在 Windows 平台显示的中文内容，其字号应不小于 12px（由于 Windows 的字体渲染机制，小于 12px 的文字显示效果极差）
- font-weight 属性必须使用数值方式描述

```
/* good */
h1 {
    font-weight: 700;
}

/* bad */
h1 {
    font-weight: bold;
}
```

------------

带私有前缀的属性由长到短排列，按冒号位置对齐。

标准属性放在最后，按冒号对齐方便阅读，也便于在编辑器内进行多行编辑。

示例：

```
.box {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}
```

### JavaScript

- **JavaScript 应当放在页面末尾，或采用异步加载（将 script 放在页面中间将阻断页面的渲染）**

