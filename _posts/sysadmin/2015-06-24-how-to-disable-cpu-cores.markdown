---
layout: post
title: "How to Disable CPU Cores"
author:
modified:
comments: true
categories: sysadmin
excerpt: "How to Disable CPU Cores In Linux Operating System"
tags: [Linux, SysAdmin, SystemAdmin, CPU]
image:
  feature:
date: 2015-06-24T16:42:04+05:30
---

{% include _toc.html %}

### CPU Information
* I've Intel(R) Core(TM) i7-3770 CPU @ 3.40GHz processor, Which has 8 cores.

{% highlight bash %}
[mitesh@Matrix ~]$ cat /proc/cpuinfo | grep processor
processor	: 0
processor	: 1
processor	: 2
processor	: 3
processor	: 4
processor	: 5
processor	: 6
processor	: 7
{% endhighlight %}

### Check Online Cores

{% highlight bash %}
[mitesh@Matrix ~]$ cat /sys/devices/system/cpu/online
0-7
{% endhighlight %}

**NOTE!**: All cores from 0 to 7 are online.
{: .notice}

### Disable core 6 and core 7
{% highlight bash %}
[mitesh@Matrix ~]$ sudo sh -c "echo 0 > /sys/devices/system/cpu/cpu6/online"
[mitesh@Matrix ~]$ sudo sh -c "echo 0 > /sys/devices/system/cpu/cpu7/online"
{% endhighlight %}

**NOTE!**: You can check cpu cores using `htop` command or `cat /sys/devices/system/cpu/online`
{: .notice}
