---
layout: post
title: How to Fix Charles Proxy Crash/Hang Issue
author:
modified:
comments: true
categories: linux/ubuntu
excerpt: "Step by step guide to fix charles proxy hang/crash issue on Ubuntu OS"
tags: [Linux, Ubuntu, Charles Proxy]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/13756323/6a4aee9a-ea45-11e5-8789-117719037741.png
  alt: How to Fix Charles Proxy Crash
  title: How to Fix Charles Proxy Crash
  feature:
date: 2016-03-15T00:30:41+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Issue

* Few weeks back found charles proxy keep crash/hang on Ubuntu
* After analyze found that charles proxy with latest OpenJDK cause this issue

### How to Fix

* Downgrade OpenJDK

{% highlight bash %}
# Remove OpenJDK
$ sudo apt-get purge openjdk-8-jdk openjdk-8-jre*

# Download little older version of OpenJDK
$ wget http://archive.ubuntu.com/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jdk_8u45-b14-1_amd64.deb http://archive.ubuntu.com/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jre-headless_8u45-b14-1_amd64.deb http://archive.ubuntu.com/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jre-jamvm_8u45-b14-1_amd64.deb http://archive.ubuntu.com/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jre_8u45-b14-1_amd64.deb

# Install OpenJDK & Charles Proxy
$ sudo dpkg -i openjdk-8-j* && sudo apt-get install charles-proxy
{% endhighlight %}
