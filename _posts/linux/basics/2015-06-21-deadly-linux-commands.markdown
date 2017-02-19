---
layout: post
title: "Deadly Linux Commands"
author:
modified:
comments: true
categories: linux/basics
excerpt: "If you are new to Linux, chances are you will meet a stupid person perhaps in a forum or chat room that can trick you into using commands that will harm your files or even your entire operating system."
tags: [Linux, Linux Basics, Tutorials, CommandLine]
image:
  feature:
date: 2015-06-21T17:16:12+05:30
---

{% include _toc.html %}

* If you are new to Linux, chances are you will meet a stupid person perhaps in a forum or chat room that can trick you into using commands that will harm your files or even your entire operating system.
* Don't run anything without understanding exactly what it does. Look it up (use Google or `man` command) if there's any part of a command you're not clear on.

* Also you can use <a href="http://explainshell.com/"> http://explainshell.com/</a> to explain what command is.
* To avoid this dangerous scenario from happening, I have here a list of deadly (Most Dangerous) Linux commands that you should avoid

<img alt="deadly-linux-commands" src="https://lh4.googleusercontent.com/-MMmhFydgamw/Tx1OEQeyRII/AAAAAAAABKg/23kyaDUseoo/s256-no/Dead.png">

### Delete Everything Recursively

* This command will recursively and forcefully delete all the files inside the `/` root directory.
{% highlight bash %}
[mitesh@Matrix ~]$ sudo rm -rf /
{% endhighlight %}

### Fork Bomb

* The following weird looking command is actually function which created copies of itself endlessly.
* This Fork Bomb quickly use all your system resources like CPU, RAM, etc and cause system crash.
* This can often lead to corruption of data.
* For More Detailed Information Refer <a href="https://en.wikipedia.org/wiki/Fork_bomb">https://en.wikipedia.org/wiki/Fork_bomb</a>

{% highlight bash %}
[mitesh@Matrix ~]$ :(){:|:&};:
{% endhighlight %}

### Format Hard Disk Drive

* This will reformat or wipeout all the files of the device that is mentioned after the `mkfs` command.
{% highlight bash %}
[mitesh@Matrix ~]$ sudo mkfs.ext3 /dev/sda
{% endhighlight %}

### Fill Hard Disk Drive with Junk Data
{% highlight bash %}
# Fill HDD with letter y
[mitesh@Matrix ~]$ sudo yes > /dev/sda
# Fill HDD with number zero 0
[mitesh@Matrix ~]$ sudo dd if=/dev/zero of=/dev/sda
# Fill HDD with random data
[mitesh@Matrix ~]$ sudo dd if=/dev/urandom of=/dev/sda
# Overwrite HDD Data (Multiple Times)
[mitesh@Matrix ~]$ sudo shred /dev/sda
{% endhighlight %}

### Delete /etc Directory

* The following command delete all the linux configuration files.
{% highlight bash %}
# Delete /boot Directory
[mitesh@Matrix ~]$ sudo rm -rf /etc
{% endhighlight %}

### Delete Boot Entry

* The following command delete Kernel, Initrd, and GRUB/LILO Files (Needed For Linux Startup)
{% highlight bash %}
# Delete /boot Directory
[mitesh@Matrix ~]$ sudo rm -rf /boot/
{% endhighlight %}


### Find and Delete configuration files
{% highlight bash %}
[mitesh@Matrix ~]$ sudo find / -iname "*.conf" -exec rm -rf  {} \;
{% endhighlight %}

### World Writable

* The following command make your system world writable.
{% highlight bash %}
[mitesh@Matrix ~]$ sudo chmod -R 777 /
{% endhighlight %}

### Remove Access Privilege

{% highlight bash %}
[mitesh@Matrix ~]$ sudo chmod -R 000 /
# Another way
[mitesh@Matrix ~]$ sudo chown -R nobody:nobody /
{% endhighlight %}
