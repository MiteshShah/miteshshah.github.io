---
layout: post
title: How to Search/remove Google Chrome History
author:
modified:
comments: true
categories: tips-and-tricks
excerpt: "How crashchrome.com works and clear Google Chrome History"
tags: [Chrome, Browser]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/16765760/9174db2c-4853-11e6-8d4f-ee405511d366.png
  alt: How to Search/remove Google Chrome History
  title: How to Search/remove Google Chrome History
  feature:
date: 2016-07-12T11:07:23+05:30
---


{% include _toc.html %}

### Issue

* Recently, I'd open <a href="http://crashchrome.com">Crash Chrome</a>
* Now after Google Chrome crash, I'd check Chrome History and entire Chrome History is full of logs of <a href="http://crashchrome.com">Crash Chrome</a>

### How Crash Chrome Works
* <a href="http://crashchrome.com">Crash Chrome</a> is a simple redirection loop
* It's a webpage that sends you to ($webAddress + nextnumber)

* http://crashchrome.com redirect to http://crashchrome.com/1
* http://crashchrome.com/1 redirect to http://crashchrome.com/12


### Now Let's clear Chrome History

* In Chrome History there is no `select all` button
* So let's do it little different way

#### Perform Search & Remove

* Open chrome://history-frame/#q=crashchrome
* Right click on this page
* Click on Inspect Element
* Click on Console
* Copy Paste Following Code and Press Enter.

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

{% highlight text %}
$('remove-selected').removeAttribute("disabled"); Array.prototype.forEach.call(document.querySelectorAll("input[type=checkbox]"), function(node) {node.checked = "checked"}); $('remove-selected').click()
{% endhighlight %}
