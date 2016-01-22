---
layout: post
title: Sudo Insults
author:
modified:
comments: true
categories: linux/tutorials
excerpt: "An interesting sudo feature is the ability to insult you when you type a wrong password. It has really funny phrases."
tags: [Linux, Sudo, Tutorials]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/12516632/6c06708e-c154-11e5-9a0d-5a06d5cef2c7.png
  alt: Sudo Insults
  title: Sudo Insults
  feature:
date: 2016-01-22T22:05:06+05:30
---

* An interesting sudo feature is the ability to insult you when you type a wrong password.

{% highlight bash %}
$ sudo visudo
# Find the line that begins with Default and add following line
Defaults 	insults
{% endhighlight %}

* It has really funny phrases, see some of them

{% highlight bash %}
Take a stress pill and think things over.
There must be cure for it!
I’ve seen penguins that can type better than that.
Maybe if you used more than just two fingers...
I think ... err ... I think ... I think I'll go home
{% endhighlight %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
