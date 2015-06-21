---
layout: post
title: "Disable IPv6"
author:
modified:
comments: true
categories: sysadmin
excerpt: "Sometime IPv6 slow down internet connections, In this article I'll show you how to disable IPv6."
tags: [Linux, SysAdmin, SystemAdmin, IPv6]
image:
  feature:
date: 2015-06-21T12:12:47+05:30
---

{% include _toc.html %}

### Check IPv6 is Enabled/Disabled
{% highlight bash %}
[mitesh@Matrix ~]$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
{% endhighlight %}

**NOTE!** If above command return <br>
1 means ipv6 is completely disabled <br>
0 means ipv6 is not completely disabled
{: .notice}

### Disable IPv6

#### Temporary Disable IPv6
{% highlight bash %}
[mitesh@Matrix ~]$ echo "1" > /proc/sys/net/ipv6/conf/all/disable_ipv6
{% endhighlight %}

**NOTE!:** Above command disable IPv6 untill your system reboot.
{: .notice}

#### Permantly Disable IPv6

* Open `/etc/sysctl.conf` file and add following lines
{% highlight bash %}
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
{% endhighlight %}

* Now run following command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo sysctl -p
{% endhighlight %}
