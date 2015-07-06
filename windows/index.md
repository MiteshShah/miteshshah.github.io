---
layout: page
title: Windows
tags: [Microsoft, Windows, Guide, Tutorials]
modified: 2015-07-06T13:38:21+05:30
comments: true
image:
  feature:
  credit:
  creditlink:
---

{% for post in site.categories.windows %}
  <li><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
{% endfor %}

<li><a href="/how-to-findout-your-system-information/#windows"> How to Findout Your System Information </a></li>
