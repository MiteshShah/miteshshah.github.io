---
layout: post
title: How to Fix Nf_conntrack Table Full Dropping Packet
author:
modified:
comments: true
categories: linux/centos
excerpt:
tags: [Linux, CentOS ]
image:
  url: https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Centos-logo-light.svg/2000px-Centos-logo-light.svg.png
  alt: How to Fix Nf_conntrack Table Full Dropping Packet
  title: How to Fix Nf_conntrack Table Full Dropping Packet
  feature:
date: 2016-07-04T15:05:11+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Issue

Packet drops on this system for connections using `ip_conntrack` or `nf_conntrack`.
Following messages seen in `/var/log/kern` on the centos nodes when one of the instances drops packets:

{% highlight bash %}
$ tail -f /var/log/kern
Jul  4 03:47:16 centos kernel: : nf_conntrack: table full, dropping packet
Jul  4 03:47:16 centos kernel: : nf_conntrack: table full, dropping packet
{% endhighlight %}

This can happen when you are being attacked, or is also very likely to happen on a busy server even if there is no malicious activity.

**NOTE:**
By default, CentOS will set this maximum to 65,536 connections.
This is enough for lightly loaded servers, but can easily be exhausted on heavy traffic servers.
{: .notice }

### How to Fix

View the current maximum configured connections

{% highlight bash %}
$ cat /proc/sys/net/netfilter/nf_conntrack_max
{% endhighlight %}


To see the current used connections

{% highlight bash %}
$ cat /proc/sys/net/netfilter/nf_conntrack_count
{% endhighlight %}

Increase maximum configured connections limit

{% highlight bash %}
# Temporarily Solution
echo 524288 > /proc/sys/net/netfilter/nf_conntrack_max


# Permanent Solution
# Add following line on /etc/rc.d/rc.local

$ vim /etc/rc.d/rc.local
echo 524288 > /proc/sys/net/netfilter/nf_conntrack_max

$ chmod a+x /etc/rc.d/rc.local
{% endhighlight %}
