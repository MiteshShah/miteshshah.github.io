---
layout: post
title: "Run Commands Without Login Into User Shell"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "How to execute commands Without Login into user shell"
tags: [Linux, CommandLine, CommandOfTheDay, Bash, Sudo]
image:
  feature:
date: 2015-07-08T20:33:33+05:30
---

{% include _toc.html %}

* Mostly While setting up web server, I was run multiple commands as root user.
* After that I realize I've to change ownership of web root :(
* By using following command we can directly create directory, copy files with given username instead of root user.

### Syntax

{% highlight bash %}
$ sudo -H -u <USERNAME> <COMMAND>
{% endhighlight %}

### Examples

{% highlight bash %}
$ sudo -H -u mitesh mkdir ~mitesh/bin
$ sudo ls -ld ~mitesh/bin
drwxr-xr-x 2 mitesh mitesh 4096 Jul  8 20:32 /home/mitesh/bin
{% endhighlight %}
