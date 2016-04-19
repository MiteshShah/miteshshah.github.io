---
layout: post
title: How to Block Facebook
author:
modified:
comments: true
categories: sysadmin
excerpt: "Sometimes, We need to block Social Networking websites like Facebook to deal with slow internet."
tags: [IPTABLE, Facebook, SysAdmin]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9797567/756886b6-5819-11e5-9a1f-d652b4237eec.png
  alt: Block Facebook
  title: Block Facebook
  feature:
date: 2015-09-11T00:03:51+05:30
---

{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Find Facebook Subnets

{% highlight bash %}
$ whois -h whois.radb.net '!gAS32934'
{% endhighlight %}

### Block Facebook

{% highlight bash %}
#!/bin/bash
for ip in `whois -h whois.radb.net '!gAS32934' | grep /`
do
  iptables -A FORWARD -p all -d $ip -j REJECT
done
{% endhighlight %}
