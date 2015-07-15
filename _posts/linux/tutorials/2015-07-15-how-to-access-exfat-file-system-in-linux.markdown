---
layout: post
title: "How to Access exFAT File System in Linux"
author:
modified:
comments: true
categories: linux/tutorials
excerpt: "Linux doesn't provide native support to exFAT File System. In this tutorial I'll show you how to access the exFAT File System in Linux."
tags: [Linux, Ubuntu, exFAT, Tutorials,]
image:
  feature:
date: 2015-07-15T15:05:27+05:30
---

{% include _toc.html %}

* Linux doesn't provide native support to exFAT File System
* If you want to access exFAT File System, You have to install following packages.

{% highlight bash %}
$ sudo apt-get install exfat-fuse exfat-utils
{% endhighlight %}
