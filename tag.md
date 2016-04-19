---
layout: page
title: Search By Tags
permalink: /tag/

sitemap:
  priority: .5

---

Click on a tag to see relevant list of posts.

<ul class="taglist">
{% for tag in site.tags %}
  {% assign t = tag | first %}
  <li class="tag"><a href="/tag/#{{t | downcase | replace:" ","-" }}">{{ t | downcase }}</a></li>
{% endfor %}
</ul>

---

{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}

<h4><a name="{{t | downcase | replace:" ","-" }}"></a><a class="internal" href="/tag/#{{t | downcase | replace:" ","-" }}">{{ t | downcase }}</a></h4>
<ul>
{% for post in posts %}
  {% if post.tags contains t %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
  {% endif %}
{% endfor %}
</ul>

---

{% endfor %}
