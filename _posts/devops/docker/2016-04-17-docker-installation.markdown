---
layout: post
title: Docker - Installation
author: Harshad_Yeola
modified:
comments: true
categories: devops/docker
excerpt: "Step by step guide to install Docker on Ubuntu, Debian & CentOS"
tags: [devops, docker]
image:
  url:
  alt: Docker - Installation
  title: Docker - Installation
  feature:
date: 2016-04-17T23:02:54+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Installation

Docker Supports almost all Linux Distributions and OS. Here are installation instruction for some popular OS Distributions. You must fullfill following Prerequisites before moving for Installation steps

#### Prerequisites
- You must have 64-bit OS version Installed on your system.
- Kernel version higher than 3.10

For Ubuntu :
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ echo "deb https://apt.dockerproject.org/repo ubuntu-${codename} main" | sudo tee -ai /etc/apt/sources.list.d/docker.list
$ sudo apt-get update
$ sudo apt-get purge lxc-docker
$ sudo apt-get install docker-engine
$ sudo service docker start
{% endhighlight %}

For Debian:

{% highlight bash %}
$ sudo apt-get purge lxc-docker*
$ sudo apt-get purge docker.io*
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ echo "deb https://apt.dockerproject.org/repo debian-${codename} main" | sudo tee -ai /etc/apt/sources.list.d/docker.list
$ sudo apt-get update
$ sudo apt-get install docker-engine
$ sudo service docker start
{% endhighlight %}

For CentOS :
{% highlight bash %}
$ sudo yum update
$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
$ sudo yum install docker-engine
$ sudo service docker start
{% endhighlight %}

We will discuss in next tutorials using Docker for your app development and deployment.
