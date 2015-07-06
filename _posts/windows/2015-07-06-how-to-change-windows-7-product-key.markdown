---
layout: post
title: "How to Change Windows 7 Product Key"
author:
modified:
comments: true
categories: windows
excerpt: "How to Change Windows 7 Product Key or Activate Windows Using Legal Key"
tags: []
image:
  feature:
date: 2015-07-06T13:38:21+05:30
---
{% include _toc.html %}

### GUI Method

* Click the Start button <i class="fa fa-windows"></i>, right-click `Computer`, and then click `Properties`.
* Scroll down to the bottom of the window that appears, and then, under `Windows activation`, click `Change product key`.
* If you're prompted for permission to continue the process, click Continue.
* Follow the instructions to change your product key and activate your copy of Windows 7.

### Command Line Method

* Click the Start button <i class="fa fa-windows"></i> and run `cmd` as Administrator.
* Type following Command
{% highlight bash %}
C:\Windows\System32> slmgr.vbs -ipk "INSERT-YOUR-PRODUCT-KEY"

# Activate Windows
C:\Windows\System32> slmgr.vbs -ato
{% endhighlight %}

<img src="https://cloud.githubusercontent.com/assets/1223371/8518251/ec0e7efa-23e5-11e5-87a0-9e4a88499247.png">
