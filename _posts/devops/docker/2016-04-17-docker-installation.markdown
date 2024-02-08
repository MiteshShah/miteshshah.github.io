---
layout: post
title: Docker - Installation
author: Harshad_Yeola
modified:
comments: true
categories: devops/docker
excerpt: "Step by step guide to install Docker on Ubuntu, Debian ,CentOS & Mac"
tags: [Devops, Docker, Ubuntu, Debian, MAC OS X, Homebrew]
image:
  url:
  alt: Docker - Installation
  title: Docker - Installation
  feature:
date: 2016-04-17T23:02:54+05:30
---


{% include _toc.html %}


### Prerequisites
- Kernel version higher than 3.10
- You must have 64-bit OS version Installed on your system.

### Installation

Docker Supports almost all Linux Distributions and OS. Here are installation instruction for some popular OS Distributions. You must fulfill following Prerequisites before moving for Installation steps

#### Install Docker on Ubuntu
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ sudo echo "deb https://apt.dockerproject.org/repo ubuntu-$(lsb_release -cs) main" > /etc/apt/sources.list.d/docker.list

$ sudo apt-get update
$ sudo apt-get purge lxc-docker
$ sudo apt-get install docker-engine
$ sudo service docker start
{% endhighlight %}


#### Install Docker on Mac OS X
{% highlight bash %}
$ brew install --cask docker
{% endhighlight %}

**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}

**NOTE!:** tcp://IP:PORT You have to replace IP:PORT given by above command
{: .notice }
