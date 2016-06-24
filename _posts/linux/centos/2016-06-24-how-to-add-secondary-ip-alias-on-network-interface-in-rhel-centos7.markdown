---
layout: post
title: How to Add Secondary IP/Alias on Network Interface in RHEL/CentOS7
author:
modified:
comments: true
categories: linux/centos
excerpt: "This guide will show you how to add an extra IP address to an existing interface in Red Hat Enterprise Linux / CentOS 7."
tags: [Linux, CentOS]
image:
  url:
  alt: How to Add Secondary IP/Alias on Network Interface in RHEL/CentOS7
  title: How to Add Secondary IP/Alias on Network Interface in RHEL/CentOS7
  feature:
date: 2016-06-24T12:06:42+05:30
---


{% include _toc.html %}


### Manually Configure Network Interface

{% highlight bash %}
$  cat /etc/sysconfig/network-scripts/ifcfg-eth0\:1
NM_CONTROLLED="no"
DEVICE="eth0:1"
ONBOOT="yes"
BOOTPROTO="static"
IPADDR="192.168.198.60"
NETMASK="255.255.128.0"
{% endhighlight %}

### Bring up Network Alias
{% highlight bash %}
$ ifup eth0:1
{% endhighlight %}


### Save Setting Permantly
{% highlight bash %}
$ echo "ifup eth0:1" >> /etc/rc.local
$ sudo chmod a+x /etc/rc.d/rc.local
{% endhighlight %}
