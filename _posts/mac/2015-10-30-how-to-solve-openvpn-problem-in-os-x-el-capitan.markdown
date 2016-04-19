---
layout: post
title: How to Solve OpenVPN Problem in OS X El Capitan
author:
modified:
comments: true
categories: mac
excerpt: " If you are getting an error like `DynamicClientBase: JSONDialog: Error running jsondialog, status`"
tags: [MAC OS X, El Capitan, OpenVPN]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/10842475/d9b37bb0-7f17-11e5-8aae-f22b81fdf9c6.png
  alt: How to Solve OpenVPN Problem in OS X El Capitan
  title: How to Solve OpenVPN Problem in OS X El Capitan
  feature:
date: 2015-10-30T14:52:26+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### How to fix JSONDialog Error
* If you are getting an error like `DynamicClientBase: JSONDialog: Error running jsondialog, status`
* You need to disable System Integrity Protection in OS X El Capitan.

### Disable System Integrity Protection in OS X El Capitan

* Reboot the Mac and hold down `Command + R` keys simultaneously after you hear the startup chime, this will boot OS X into Recovery Mode.
* When the `OS X Utilities` screen appears, pull down the `Utilities` menu at the top of the screen instead, and choose `Terminal`
* Type the following command into the terminal then hit return.

{% highlight bash %}
$ csrutil disable; reboot
{% endhighlight %}
