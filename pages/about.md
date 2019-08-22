---
layout: page
title: About
description: About Me
keywords: Jcohy, Jiac
comments: true
menu: 关于
permalink: /about/
---

越是错综复杂的问题。

就越要根据简单的原理和朴素的思想进行判断和行动。

我想这是拨开云雾见南山，直接洞悉事物本质和解决问题的最佳方法。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[{{ website.name }}]({{ website.url }})
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
