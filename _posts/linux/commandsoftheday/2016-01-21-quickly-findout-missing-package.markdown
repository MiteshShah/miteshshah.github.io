---
layout: post
title: Quickly Findout Missing Package
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Sometimes we need to check installed software on multiple system and install missing software asap."
tags: [Linux, Ubuntu, Debian, CLI, CommandLine]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/12490016/6ed4aa62-c099-11e5-8807-156d513ce75f.png
  alt: Quickly Findout Missing Package
  title: Quickly Findout Missing Package
  feature:
date: 2016-01-21T23:47:23+05:30
---


* Sometimes we need to check installed software on multiple system and install missing software asap.

{% highlight bash %}
# Debin/Ubuntu
$ dpkg-query -Wf='${package}\n' | grep php

# Redhat/CentOS
$ rpm -qa --qf "%{NAME}\n" | grep php
{% endhighlight %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

**NOTE:** Use `diff` command for quickly findout missing package.
{: .notice}
