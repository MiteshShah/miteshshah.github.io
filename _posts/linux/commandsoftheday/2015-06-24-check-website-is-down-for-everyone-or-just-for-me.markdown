---
layout: post
title: "Check Website Is Down for Everyone or Just for Me"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Sometimes we need to check weather some Website is down for everyone or It's just for me."
tags: [Linux, CommandLine, CommandOfTheDay, Curl]
image:
  feature:
date: 2015-06-24T20:48:24+05:30
---

{% include _toc.html %}

### Check Online

* Open <a href="http://isup.me"> http://isup.me </a>
* Enter the Domain name you want to check.

### Check from Command Lines
{% highlight text %}
[mitesh@Matrix ~]$ curl -s www.isup.me/google.com | grep '"container"' -A1| sed 's:<.*::'
It's just you.
[mitesh@Matrix ~]$ curl -s www.isup.me/example.com | grep '"container"' -A1| sed 's:<.*::'
It's not just you!
{% endhighlight %}
