---
layout: page
title: About
description: 关于张拭心
keywords: 张拭心, shixinzhang, about me
comments: true
menu: 关于
permalink: /about/
---

我是张拭心，“拭心” 意为：时常拂拭内心，避免误入歧途。

目前为安卓开发者，业余也学点前端。

技术文章目前主要发表在 CSDN：<http://blog.csdn.net/u011240877>

生活相关发表在 简书：<http://www.jianshu.com/u/8e8ad16d1fcf>

欢迎扫码关注我的公众号“安卓进化论”，分享技术和人生思考。

![](http://img.blog.csdn.net/20160923012706321)

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
