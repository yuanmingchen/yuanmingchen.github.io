---
layout: page
title: 笔记
description: 学习笔记
keywords: note,笔记
comments: false
permalink: /note/
---

<ul class="listing">
{% for note in site.categories.note %}
{% if note.title != "note Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ note.url }}">{{ note.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
