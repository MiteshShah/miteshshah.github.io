---
layout: post
title: "How to Flush DNS Cache on Mac OS"
author:
modified:
comments: true
categories: mac
excerpt: "How to Flush DNS Cache on Mac OS"
tags: [MAC, OS X, Tips, Tricks, Terminal, DNS]
image:
  feature:
date: 2015-07-03T16:53:49+05:30
---

* Open Terminal
* Type Following command

{% highlight bash %}
$ dscacheutil -flushcache; sudo killall -HUP mDNSResponder
{% endhighlight %}

**NOTE!**: Above command ask for the admin password.
{: .notice}
