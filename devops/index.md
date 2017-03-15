---
layout: page
title: DevOps
tags: [DevOps]
modified: 2017-03-15T12:57:12+05:30
comments: true
image:
  feature:
  credit:
  creditlink:
---

{% for post in site.categories.devops %}
  <li><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
{% endfor %}

<li><a href="/devops/aws"> AWS </a></li>
<li><a href="/devops/nagios"> Nagios </a></li>
<li><a href="/devops/ansible"> Ansible </a></li>
<li><a href="/devops/docker"> Docker </a></li>
<li><a href="/linux/elk"> ELK Stack </a></li>
<li><a href="/devops/lemp"> LEMP Stack </a></li>
