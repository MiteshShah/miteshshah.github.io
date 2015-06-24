---
layout: post
title: "Access Website Without Editing DNS"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Simple curl command which is useful when we want to test staging website without edit /etc/host file or DNS"
tags: [Linux, CommandLine, CommandOfTheDay, DNS]
image:
  feature:
date: 2015-06-16T17:59:41+05:30
---

### Access Website Without Editing DNS

* Simple `curl` command which is useful when we want to test staging website without edit /etc/host file or DNS

{% highlight bash %}
[mitesh@Matrix ~]$ curl -H 'Host: example.com' 192.168.33.10:80
{% endhighlight %}
