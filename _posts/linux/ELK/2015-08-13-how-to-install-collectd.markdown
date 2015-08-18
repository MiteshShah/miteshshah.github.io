---
layout: post
title: How to Install Collectd
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to install Collectd – The system statistics collection daemon"
tags: [Linux, ELK, Collectd]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-13T14:19:46+05:30
---

{% include _toc.html %}

### Collectd

* Collectd – The system statistics collection daemon.
* collectd gathers statistics about the system it is running on and stores this information.
* Those statistics can then be used to find current performance bottlenecks (i.e. performance analysis) and predict future system load (i.e. capacity planning)

### Install Collectd

### Install Collectd on Debian/Ubuntu
{% highlight bash %}
$ sudo apt-get install collectd collectd-utils
{% endhighlight %}

### Install Collectd on CentOS
{% highlight bash %}
$ sudo yum install epel-release
$ sudo yum install collectd
{% endhighlight %}
