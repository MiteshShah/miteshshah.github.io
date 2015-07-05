---
layout: post
title: "How to Edit the Hosts File on Linux"
author:
modified:
comments: true
categories: linux/tutorials
excerpt: "The Hosts file is used to map Hostname to IP address."
tags: [Linux, Tutorials, Hosts, DNS]
image:
  feature:
date: 2015-07-05T13:29:03+05:30
---

{% include _toc.html %}

### Edit Hosts File Using Terminal

* Open Terminal (Ctrl + Alt + T)
* Run following command

{% highlight bash %}
$ sudo vim /etc/hosts
Password:
{% endhighlight %}

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


**NOTE!**: If you don't know how to use VIM, You can check <a href="/linux/basics/vim-an-advance-text-editor/">VIM - An Advance Text Editor</a>
{: .notice}
