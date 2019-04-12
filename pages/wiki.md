---
layout: page
title: 维基
description: 人越学越觉得自己无知
keywords: 维基, Wiki
comments: false
permalink: /wiki/
---

<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
