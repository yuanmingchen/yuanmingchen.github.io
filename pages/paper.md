---
layout: page
title: 论文
tagline: 论文笔记
keywords: 论文, paper
comments: true
permalink: /paper/
---

<ul class="listing">
{% for paper in site.paper %}
{% if paper.title != "paper Template" %}
	<li class="listing-item"><a href="{{ site.url }}{{ paper.url }}">{{ paper.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
