---
layout: post
title: "How to Findout Number of Available Processors"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Findout how many Processors available on your system/servers using the simple command nproc."
tags: [Linux, CommandLine, CommandOfTheDay, Nproc]
image:
  url:
  alt:
  title:
  feature:
date: 2015-07-21T15:17:05+05:30
---

#### Findout Available Processors
{% highlight bash %}
$ grep processor /proc/cpuinfo | wc -l
32
{% endhighlight %}

{% highlight bash %}
$ nproc
32
{% endhighlight %}
