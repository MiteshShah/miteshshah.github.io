---
layout: post
title: How to Fix Mac OS Sierra - the Installer Payload Failed Signature Check
author:
modified:
comments: true
categories: mac
excerpt:
tags: [MAC OS X, Bootable, Sierra]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/22148225/dd6d4868-df32-11e6-8fc3-9f57783a015a.png
  alt: How to Fix Mac OS Sierra - the Installer Payload Failed Signature Check
  title: How to Fix Mac OS Sierra - the Installer Payload Failed Signature Check
  feature:
date: 2017-01-20T17:02:33+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### ISSUE

* The installer checks the date on the computer.
* If the date isn’t current, you get the error above.
* The fix involves correcting the date on your Mac.

### How to Fix

* If you use an external boot disk, your Mac starts up into OS X Disk Utilities.
* You can access the Terminal by clicking on the Utilities menu and selecting Terminal.
* Once the Terminal has launched, follow these steps.

{% highlight bash %}
$ ntpdate -u time.apple.com
{% endhighlight %}
