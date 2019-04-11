---
layout: page
title: 收藏
description: 网址收藏
keywords: 收藏, collection
comments: false
permalink: /collect/
---

<ul class="listing">
{% for collect in site.collect %}
{% if collect.title != "collect Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ collect.url }}">{{ collect.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
