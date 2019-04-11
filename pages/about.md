---
layout: page
title: 关于
description: 关于
keywords: 关于, about
comments: false
permalink: /about/
---
#### 袁明琛，哈尔滨工业大学在读菜鸡一个，学习自然语言处理，主要学习情感分析方向。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[{{ website.name }}]({{ website.url }})
{% endfor %}
