---
layout: post
title: How to Fix No Space Left on Device
author:
modified:
comments: true
categories: sysadmin
excerpt: "How to fix running out of inodes issue"
tags: [SysAdmin, HDD, Disk Space, Inodes, FileSystem]
image:
  url: https://upload.wikimedia.org/wikipedia/commons/c/c7/HuaweiRH2288HV2.JPG
  alt: How to Fix No Space Left on Device
  title: How to Fix No Space Left on Device
  feature:
date: 2016-08-01T16:52:45+05:30
---


{% include _toc.html %}

### Issue

* One of our servers went down today.
* Problem started with developer complains about "No space left on device"

* I'd checked server and found server has nearly 60% free disk space.

{% highlight bash %}
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        24G  7.7G   15G  35% /
devtmpfs        998M  4.0K  998M   1% /dev
none            4.0K     0  4.0K   0% /sys/fs/cgroup
none            200M  352K  200M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none           1000M     0 1000M   0% /run/shm
none            100M     0  100M   0% /run/user
{% endhighlight %}

* Let's check available Inodes
{% highlight bash %}
$ df -i
Filesystem      Inodes   IUsed  IFree IUse% Mounted on
/dev/root      1504000 1504000 0       100% /
devtmpfs        255453    1383 254070    1% /dev
none            255919       2 255917    1% /sys/fs/cgroup
none            255919     842 255077    1% /run
none            255919       1 255918    1% /run/lock
none            255919       1 255918    1% /run/shm
none            255919       2 255917    1% /run/user
{% endhighlight %}

* If you ever run into such trouble – most likely you have too many small or 0-sized files on your disk
* while you have enough free disk space, you have exhausted all available Inodes.

### How to Fix

#### Find those small files

{% highlight bash %}
$ sudo for i in /*; do echo $i; find $i | wc -l; done
{% endhighlight %}

* This command will list directories and number of files in them.
* Once you see a directory with unusually high number of files (or command just hangs over calculation for a long time), repeat the command for that directory to see where exactly the small files are

{% highlight bash %}
$ for i in /home/*; do echo $i; find $i | wc -l; done
{% endhighlight %}

* Once you found the suspect – just delete the files

{% highlight bash %}
$ sudo rm -rf /home/bad_user/lots_of_small_junk_files
{% endhighlight %}

#### Let's Cross Check


{% highlight bash %}
$ df -i
Filesystem      Inodes   IUsed  IFree IUse% Mounted on
/dev/root      1504000 1132528 371472   76% /
devtmpfs        255453    1383 254070    1% /dev
none            255919       2 255917    1% /sys/fs/cgroup
none            255919     842 255077    1% /run
none            255919       1 255918    1% /run/lock
none            255919       1 255918    1% /run/shm
none            255919       2 255917    1% /run/user
{% endhighlight %}
