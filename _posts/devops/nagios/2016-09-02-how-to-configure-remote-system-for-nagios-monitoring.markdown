---
layout: post
title: How to Configure Remote System for Nagios Monitoring
author:
modified:
comments: true
categories: devops/nagios
excerpt: "Step by step guide to add remote server on Nagios System"
tags: [Devops, Nagios, NRPE, Ubuntu]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/18166139/0917267c-7067-11e6-8efb-58950b52fab4.jpg
  alt: How to Configure Remote System for Nagios Monitoring
  title: How to Configure Remote System for Nagios Monitoring
  feature:
date: 2016-09-02T00:52:33+05:30
---


{% include _toc.html %}

### Prerequisites

{% highlight bash %}
# Ubuntu/Debian
$ sudo apt-get install wget build-essential openssl libssl-dev
{% endhighlight %}

### Adding the Nagios User and Group
{% highlight bash %}
$ useradd nagios
$ groupadd nagcmd
$ usermod -a -G nagcmd nagios
$ usermod -a -G nagios,nagcmd www-data
{% endhighlight %}

### Download Nagios Plugins & NRPE Tarballs
{% highlight bash %}
$ cd /tmp
$ wget http://nagios-plugins.org/download/nagios-plugins-2.1.2.tar.gz
$ wget https://github.com/NagiosEnterprises/nrpe/archive/3.0.tar.gz
{% endhighlight %}

#### Extract Nagios Plugins & NRPE Tarballs
{% highlight bash %}
$ tar zxvf nagios-plugins-2.1.2.tar.gz
$ tar zxvf 3.0.tar.gz
{% endhighlight %}

### Nagios Plugins Installation

{% highlight bash %}
$ cd /tmp/nagios-plugins-2.1.2
$ ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-openssl
$ make
$ make install
{% endhighlight %}


### Setup NRPE
{% highlight bash %}
$ cd /tmp/nrpe-3.0/
$ ./configure
$ make all
$ make install
$ make install-config
$ make install-init

{% endhighlight %}

### Restart Services

{% highlight bash %}
$ service nagios restart
$ service nrpe restart
{% endhighlight %}


### Check NRPE

{% highlight bash %}
$ /usr/local/nagios/libexec/check_nrpe -H 127.0.0.1
NRPE vnrpe-3.0
{% endhighlight %}
