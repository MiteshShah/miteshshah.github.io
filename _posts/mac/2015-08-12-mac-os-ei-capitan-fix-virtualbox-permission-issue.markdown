---
layout: post
title: MAC OS EI Capitan - Fix Virtualbox Permission Issue
author:
modified:
comments: true
categories: mac
excerpt: "How to fix Virtualbox Permission issue on MAC OS EI Capitan"
tags: [MAC, OSX, Virtualbox]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-12T13:07:19+05:30
---
{% include _toc.html %}

### VirtualBox Permission Issue

{% highlight bash %}
Effective UID is not root (euid=501 egid=20 uid=501 gid=20) (rc=-20)
Please try reinstalling VirtualBox.
{% endhighlight %}

* On MAC OS EI Capitan Virtualbox is not installed properly and through above errors.

### How to fix VirtualBox Permission Issue

* To resolve this on El Capitan when using Virtualbox versions lower than 6.x run the following from terminal

{% highlight bash %}
#!/bin/bash
for bin in VirtualBox VirtualBoxVM VBoxNetAdpCtl VBoxNetDHCP VBoxNetNAT VBoxHeadless; do
    sudo chmod u+s "/Applications/VirtualBox.app/Contents/MacOS/${bin}"
done
{% endhighlight %}
