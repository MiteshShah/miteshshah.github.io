---
layout: post
title: Repair/Fix MAC HFS+ Partition Using Ubuntu
author:
modified:
comments: true
categories: mac
excerpt:
tags: [MAC, OSX, HFS+, Recovery, Ubuntu]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/10535240/af629b30-73fe-11e5-91ae-2dfd498986f9.png
  alt: Repair/Fix MAC HFS+ Partition Using Ubuntu
  title: Repair/Fix MAC HFS+ Partition Using Ubuntu
  feature:
date: 2015-10-16T12:05:16+05:30
---


{% include _toc.html %}

### Requirements

* Internet Connection
* Ubuntu Live USB/CD/DVD

### Data Recovery Process

* Boot From Ubuntu Live USB/CD/DVD
* Connect to Internet
* Enable Universe packages using `Software Sources`
* Install `hfsprogs` packages
{% highlight bash %}
$ sudo apt-get install hfsprogs
{% endhighlight %}

#### Find MAC OSX HFS+ Partition

* Use fdisk command to find the Mac device.
{% highlight bash %}
$ sudo fdisk -l

Disk /dev/sda: 465.8 GiB, 500107862016 bytes, 976773168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 00006451-5A98-0000-F944-0000E3560000

Device         Start       End   Sectors   Size Type
/dev/sda1         40    409639    409600   200M EFI System
{% endhighlight %}

**NOTE!:** using `fdisk` didn’t show the Main Partition of mac, It shows only one partition.<br>
The above partition `sda1` is the EFI partition (boot partition).<br>
That means, `/dev/sda2` is your Mac partition.
{:.notice}

#### Check/Repair MAC OSX HFS+ Partition

* You need to do fsck check on your Mac partition

{% highlight bash %}
$ sudo fsck.hfsplus /dev/sda2
** /dev/sda2
** Checking HFS Plus volume.
** Checking Extents Overflow file.
** Checking Catalog file.
** Checking multi-linked files.
** Checking Catalog hierarchy.
** Checking Extended Attributes file.
** Checking volume bitmap.
Volume Bit Map needs minor repair
** Checking volume information.
** Repairing volume.
** Rechecking volume.
** Checking HFS Plus volume.
** Checking Extents Overflow file.
** Checking Catalog file.
** Checking multi-linked files.
** Checking Catalog hierarchy.
** Checking Extended Attributes file.
** Checking volume bitmap.
** Checking volume information.
** The volume Macintosh HD was repaired successfully.
{% endhighlight %}

* Hurray! It is repaired now…
* Now you can copy your files using Ubuntu File Manager.
