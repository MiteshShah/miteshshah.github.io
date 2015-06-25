---
layout: post
title: "Things to Do After Installing Ubuntu Desktop"
author:
modified:
comments: true
categories: linux/Ubuntu
excerpt: "Newbie Guide - Install Git, OpenSSH-Server, Java, Shutter, Hipchat, VLC and Google Chrome"
tags: [Linux, Ubuntu]
image:
  feature:
date: 2015-06-15T13:07:29+05:30
---

{% include _toc.html %}

### Update Ubuntu System
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get -y upgrade
{% endhighlight %}

### Display startup items

* By Default, Some of startup items are hidden
* By using following command, we can unhide all startup items.

{% highlight bash %}
[mitesh@Matrix ~]$ sudo sed -i "s/NoDisplay=true/NoDisplay=false/g" /etc/xdg/autostart/*.desktop
{% endhighlight %}

### Manually Install Software

#### Install curl command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y curl
{% endhighlight %}

#### Install wget command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y wget
{% endhighlight %}

#### Install GIT command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y git-core
{% endhighlight %}

#### Install OpenSSH-Server
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y openssh-server
{% endhighlight %}

#### Install Java
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y openjdk-8-jre openjdk-8-jdk
{% endhighlight %}

#### Install Shutter

* Shutter is a feature-rich screenshot program for Linux based operating systems such as Ubuntu.
* You can take a screenshot of a specific area, window, your whole screen, or even of a website – apply different effects to it, draw on it to highlight points, and then upload to an image hosting site, all within one window.

{% highlight bash %}
[mitesh@Matrix ~]$ sudo add-apt-repository -y ppa:shutter/ppa
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y shutter
{% endhighlight %}

#### Install Hipchat
{% highlight bash %}
[mitesh@Matrix ~]$ sudo echo "deb http://downloads.hipchat.com/linux/apt stable main" > /etc/apt/sources.list.d/atlassian-hipchat.list
[mitesh@Matrix ~]$ wget -O - https://www.hipchat.com/keys/hipchat-linux.key | apt-key add -
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y hipchat
{% endhighlight %}

#### Install Skype
{% highlight bash %}
[mitesh@Matrix ~]$ sudo sh -c  'echo "deb http://archive.canonical.com/ubuntu/ $(lsb_release -sc) partner" >> /etc/apt/sources.list.d/canonical_partner.list'
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y skype sni-qt sni-qt:i386
{% endhighlight %}

#### Install VLC

* VLC is a free and open source cross-platform multimedia player and framework that plays most multimedia files as well as DVDs, Audio CDs, VCDs, and various streaming protocols.

{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y vlc
{% endhighlight %}

#### Install Google Chrome

{% highlight bash %}
[mitesh@Matrix ~]$ sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
[mitesh@Matrix ~]$ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list'
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y google-chrome-stable
{% endhighlight %}


#### Install Diodon

* Diodon is a simple clipboard manager for GNOME with application indicator support. Aiming to be the best integrated clipboard manager for the Gnome/GTK+ desktop.
{% highlight bash %}
[mitesh@Matrix ~]$ sudo add-apt-repository ppa:diodon-team/stable
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y diodon
{% endhighlight %}

#### Install GParted

* GParted is a free partition editor for graphically managing your disk partitions.
* With GParted you can resize, copy, and move partitions without data loss, enabling you to:
  * Grow or shrink your C: drive
  * Create space for new operating systems
  * Attempt data rescue from lost partitions
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get install -y gparted
{% endhighlight %}

#### Install Caffeine

* Caffeine is a tiny program that puts an icon in the right side of your menu bar.
* Click it to prevent your Ubuntu from automatically going to sleep, dimming the screen or starting screen savers.
* Click it again to go back.
* Right-click the icon to show the menu.

{% highlight bash %}
[mitesh@Matrix ~]$ sudo add-apt-repository ppa:caffeine-developers/ppa
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install caffeine
{% endhighlight %}

### Automate Above Install Steps

* All above install steps can be automate in following shell scripts.

{% highlight bash %}
[mitesh@Matrix ~]$ wget -c https://raw.githubusercontent.com/MiteshShah/admin/master/distro/ubuntu/desktop-setup.sh
[mitesh@Matrix ~]$ sudo bash desktop-setup.sh
{% endhighlight %}


The installation may take a little time, so grab a cup of coffee <i class="fa fa-coffee"></i> and relax.
