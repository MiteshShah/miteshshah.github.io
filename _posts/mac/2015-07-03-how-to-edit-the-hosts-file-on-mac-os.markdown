---
layout: post
title: "How to Edit the Hosts File on Mac OS"
author:
modified:
comments: true
categories: mac
excerpt: "The Hosts file is used to map Hostname to IP address."
tags: [MAC, OS X, Tips, Tricks, Terminal]
image:
  feature:
date: 2015-07-03T16:00:08+05:30
---

{% include _toc.html %}

### Edit Hosts File Using Terminal

* Open Terminal
* Run following command

{% highlight bash %}
$ sudo vim /private/etc/hosts
Password:
{% endhighlight %}

**NOTE!**: If you don't know how to use VIM, You can check <a href="/linux/basics/vim-an-advance-text-editor/">VIM - An Advance Text Editor</a> Guide.
{: .notice}

### Edit Hosts File Using Text Editor

* Open Finder
* Click on Finder's Menubar
* Click on Go > Go to Folder

<img src="https://cloud.githubusercontent.com/assets/1223371/8497943/1bc287d4-219f-11e5-8cfd-b0a1c801440b.png">

* In the box, type `/private/etc/hosts` location and press Enter.
<img src="https://cloud.githubusercontent.com/assets/1223371/8497942/1bc0d2a4-219f-11e5-9e2d-2775cb4e2844.jpg">

* Right on `hosts` file and open with your favorite text editor.
{% highlight bash %}
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost

192.168.0.100   miteshshah.github.io
{% endhighlight %}
