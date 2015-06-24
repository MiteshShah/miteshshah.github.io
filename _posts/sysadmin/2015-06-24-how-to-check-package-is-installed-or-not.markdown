---
layout: post
title: "How to Check Package Is Installed or Not"
author:
modified:
comments: true
categories: sysadmin
excerpt: "As a System Admin, I've to check some Package is installed or not. In this article I'll show you how to properly check some package is installed on your system or not."
tags: [Linux, SysAdmin, SystemAdmin, Debian, Ubuntu]
image:
  feature:
date: 2015-06-24T23:40:22+05:30
---

{% include _toc.html %}

* As a System Admin, I've to check some Package is installed or not.
* In this article I'll show you how to properly check some package is installed on your system or not.

### Install htop Package
{% highlight bash %}
[mitesh@Matrix ~]$ apt-get install htop
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  htop
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 68.0 kB of archives.
After this operation, 188 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu/ trusty/universe htop amd64 1.0.2-3 [68.0 kB]
Fetched 68.0 kB in 0s (98.6 kB/s)
Selecting previously unselected package htop.
(Reading database ... 86712 files and directories currently installed.)
Preparing to unpack .../htop_1.0.2-3_amd64.deb ...
Unpacking htop (1.0.2-3) ...
Processing triggers for mime-support (3.54ubuntu1.1) ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Setting up htop (1.0.2-3) ...
{% endhighlight %}

### Remove htop Package

* After install the htop package, sometimes we don't like installed package
* Let's remove the htop package

{% highlight bash %}
[mitesh@Matrix ~]$ apt-get remove htop
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages will be REMOVED:
  htop
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 188 kB disk space will be freed.
Do you want to continue? [Y/n] y
(Reading database ... 86722 files and directories currently installed.)
Removing htop (1.0.2-3) ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Processing triggers for mime-support (3.54ubuntu1.1) ...
{% endhighlight %}

### Check Package Is Installed or Not

* Most of SystemAdmin (Including Me), First try following commands

{% highlight bash %}
[mitesh@Matrix ~]$ dpkg -l | grep htop
rc  htop                             1.0.2-3                          amd64        interactive processes viewer
[mitesh@Matrix ~]$ echo $?
0
{% endhighlight %}

**NOTE!**: The problem with `dpkg -l` command is its return 0 even if package is removed from the system.
{: .notice}

* Let's fix the `dpkg -l` command to get only installed package.

{% highlight bash %}
[mitesh@Matrix ~]$ dpkg --get-selections | grep -v deinstall | grep htop
[mitesh@Matrix ~]$ echo $?
1
{% endhighlight %}

**NOTE!**: As above command return 0 only if that package is installed on your system. <br>
{: .notice}
