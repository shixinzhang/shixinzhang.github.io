---
layout: page
title: About
description: 关于张拭心
keywords: 张拭心, shixinzhang, about me
comments: true
menu: 关于
permalink: /about/
---

你好我是张拭心，感谢上帝让我们有缘在网络相遇。

内蒙古人/安卓开发者/爱写作，业余也学点前端。

目前任职于上海喜马拉雅FM，同道中人沟通或者内推欢迎加我微信：sxkejinet

技术文章目前主要发表在 CSDN：<http://blog.csdn.net/u011240877>

生活相关发表在 简书：<http://www.jianshu.com/u/8e8ad16d1fcf>

欢迎扫码关注我的公众号“zsx跃迁路”，分享技术和人生思考:

![](http://oqg4nua5z.bkt.clouddn.com/blog/0_wechat_public.png)


## 年度总结

- [2016 总结](https://blog.csdn.net/u011240877/article/details/53961190)
- [2017 总结](https://blog.csdn.net/u011240877/article/details/78967014)

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
