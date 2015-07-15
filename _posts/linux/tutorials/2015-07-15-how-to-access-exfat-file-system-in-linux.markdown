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

* Linux doesn't provide native support to exFAT File System.
* exFAT File System is useful only when you want to transfer data between multiple operating systems.
* exFat Supports Microsoft Windows, Apple Mac OS and by installing following packages exFat works with Linux.

{% highlight bash %}
$ sudo apt-get install exfat-fuse exfat-utils
{% endhighlight %}
