---
layout: post
title: "How to Findout Your System Information"
author:
modified:
comments: true
categories:
excerpt: "This article will teach you how to find out system information, such as Operating System, Serial Number, Processor, RAM and MAC Address, License Key"
tags: [System Information, Linux, MAC OS X, Windows]
image:
  feature:
date: 2015-06-17T13:53:20+05:30
---

{% include _toc.html %}

* This article will teach you how to find out system information, such as
  * Operating System (OS)
  * Serial Number
  * Processor
  * RAM
  * MAC Address


### Linux
* Find out Manufacturer, Product Name and Serial Number
{% highlight bash %}
[mitesh@Matrix ~]$ sudo dmidecode | grep -A9 'System Information'
System Information
	Manufacturer: Dell Inc.
	Product Name: Latitude 3540
	Version: A06
	Serial Number: DA3YSZ1
	UUID: 4C4B4444-0022-2210-8058-C4C04F535A31
	Wake-up Type: Power Switch
	SKU Number: Latitude 3540
	Family: Not Specified
{% endhighlight %}

* Find out Processor information
{% highlight bash %}
[mitesh@Matrix ~]$ sudo lscpu  | grep 'Model name'
Model name:            Intel(R) Core(TM) i3-4005U CPU @ 1.70GHz
{% endhighlight %}

* Find out RAM information
{% highlight bash %}
[mitesh@Matrix ~]$ sudo lshw -c memory
  *-memory
       description: System memory
       physical id: 0
       size: 3865MiB
{% endhighlight %}

* Find out WiFi Mac Address
{% highlight bash %}
[mitesh@Matrix ~]$ sudo ifconfig wlan0 | grep HWaddr
wlan0     Link encap:Ethernet  HWaddr 64:6a:05:2d:9f:6d
{% endhighlight %}

* Find out HDD Space
{% highlight bash %}
[mitesh@Matrix ~]$ sudo fdisk -l

Disk /dev/sda: 465.8 GiB, 500107862016 bytes, 976773168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 2740A572-E1AE-4B36-9F4B-2EF58DE8ECC1

Device         Start       End   Sectors   Size Type
/dev/sda1       2048   1050623   1048576   512M EFI System
/dev/sda2    1050624 968562687 967512064 461.4G Linux filesystem
/dev/sda3  968562688 976771071   8208384   3.9G Linux swap
{% endhighlight %}

* Find out LAN Mac Address
{% highlight bash %}
[mitesh@Matrix ~]$ sudo ifconfig eth0 | grep HWaddr
eth0      Link encap:Ethernet  HWaddr ed:f5:ab:9c:6c:6e
{% endhighlight %}

### MAC
* Click on Apple Menu <i class="fa fa-apple"></i>
* Click on About This Mac
* You can see the following information
  * Model Number
  * Processor
  * RAM
  * Serial Number
<img alt="About This MAC" src="https://cloud.githubusercontent.com/assets/1223371/8202592/fcf0d430-14f8-11e5-88a7-63e28e0eacf6.png">

* Click on Storage tab
  * HDD Space
<img alt="HDD"src="https://cloud.githubusercontent.com/assets/1223371/8202593/fd183214-14f8-11e5-9d9c-0a33bc6ba920.png">

* Click on Memory tab
  * RAM
<img alt="Memory" src="https://cloud.githubusercontent.com/assets/1223371/8202594/fd3e7ffa-14f8-11e5-81f3-146f61d470ff.png">

* Press `Command ⌘ and space` key
* Type `System Preferences`
* Click on Network
<img alt="Network" src="https://cloud.githubusercontent.com/assets/1223371/8202917/bfc2ddd0-14fb-11e5-9da3-ed62ae937db9.png">

* Find out WiFi Mac Address

  * Select WiFi and click on Advanced
<img alt="WIFI" src="https://cloud.githubusercontent.com/assets/1223371/8202915/bfc219f4-14fb-11e5-9d21-1bf5c51d8a46.png">

  * Click on Hardware tab
<img alt="Hardware" src="https://cloud.githubusercontent.com/assets/1223371/8202914/bfc015aa-14fb-11e5-8277-85d6f9810f41.png">

* Find out LAN Mac Address

  * Select Ethernet and click on Advanced
<img "alt=LAN" src="https://cloud.githubusercontent.com/assets/1223371/8202918/bfc2d2f4-14fb-11e5-8cbd-f98c923276b4.png">

  * Click on Hardware tab
<img alt="Hardware"src="https://cloud.githubusercontent.com/assets/1223371/8202916/bfc26706-14fb-11e5-8fba-5d86cfec1dcf.png">

### Windows

* Download and install <a href="http://www.belarc.com/de/Programs/advisorinstaller.exe"> Belarc Advisor </a>
* Once Belarc Advisor installed, its open a HTML page which contains all your system information.

<img alt="Windows System Information" src="https://cloud.githubusercontent.com/assets/1223371/8204429/3bbb4134-1506-11e5-81e7-5c505bff7a1a.png">
