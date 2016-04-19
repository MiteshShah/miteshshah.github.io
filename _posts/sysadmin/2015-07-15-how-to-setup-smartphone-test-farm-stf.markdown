---
layout: post
title: "How to Setup SmartPhone Test Farm (STF)"
author:
modified:
comments: true
categories: sysadmin
excerpt: "Smartphone Test Farm (STF) is a web application for debugging smartphones, smartwatches and other gadgets remotely, from the comfort of your browser."
tags: [Linux, SysAdmin, STF, Android, Ubuntu]
image:
  feature:
date: 2015-07-15T16:11:32+05:30
---

{% include _toc.html %}

### Smartphone Test Farm (STF)
* Smartphone Test Farm (STF) is a web application for debugging smartphones, smartwatches and other gadgets remotely, from the comfort of your browser.

<img alt="How to Setup SmartPhone Test Farm (STF) feature image" src="https://raw.githubusercontent.com/openstf/stf/master/doc/7s_usage.gif">

### Features

* Android 2.3 ~ 5.1
* Keyboard & Mouse Input
* Copy & Paste
* Take Screenshots
* Drag & Drop APK files
* Open URLs
* Display Logs
* Run Shell Commands
* Debug Remotely
* Reverse Port Forwarding, Device Rotation, Play Store Automation etc
* More Details and Features List Check - <a href="https://openstf.github.io/">https://openstf.github.io/</a>

### Setup STF On Ubuntu

{% highlight bash %}
$ curl -s https://gist.githubusercontent.com/MiteshShah/62cd0923b9068a927dae/raw/STF | sudo bash
{% endhighlight %}

**NOTE!**: Before run above command check it's code. <br>
I'll attached above shell script below.
{: .notice}

<script src="https://gist.github.com/MiteshShah/62cd0923b9068a927dae.js"></script>

### Start STF

#### Access STF Locally
{% highlight bash %}
$ sudo rethinkdb
$ sudo stf local
{% endhighlight %}

* Access STF at <a href="http://127.0.0.1:7100">http://127.0.0.1:7100</a>


#### Access STF From Another Machine
{% highlight bash %}
$ sudo rethinkdb
$ sudo stf local --public-ip <your_internal_network_ip_here>
{% endhighlight %}
