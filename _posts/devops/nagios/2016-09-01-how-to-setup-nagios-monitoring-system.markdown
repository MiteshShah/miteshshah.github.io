---
layout: post
title: How to Setup Nagios Monitoring System
author:
modified:
comments: true
categories: devops/nagios
excerpt: "Nagios is an enterprise class, open source software that can be used for network and infrastructure monitoring. Using Nagios, we can monitor servers, switches, applications and services etc. It alerts the System Administrator when something goes wrong and also alerts back when the issues have been rectified."
tags: [Devops, Nagios, NRPE, Ubuntu]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/18166139/0917267c-7067-11e6-8efb-58950b52fab4.jpg
  alt: How to Setup Nagios Monitoring System
  title: How to Setup Nagios Monitoring System
  feature:
date: 2016-09-01T17:01:36+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Prerequisites

{% highlight bash %}
# Ubuntu/Debian
sudo apt-get install wget build-essential apache2 php apache2-mod-php7.0 php-gd libgd-dev unzip apache2-utils
{% endhighlight %}

### Adding the Nagios User and Group
{% highlight bash %}
useradd nagios
groupadd nagcmd
usermod -a -G nagcmd nagios
usermod -a -G nagios,nagcmd www-data
{% endhighlight %}


### Download Nagios Core and Nagios Plugins Tarballs
{% highlight bash %}
cd /tmp
wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.2.0.tar.gz
wget http://nagios-plugins.org/download/nagios-plugins-2.1.2.tar.gz
{% endhighlight %}

#### Extract Nagios Core and Nagios Plugins Tarballs
{% highlight bash %}
tar zxvf nagios-4.2.0.tar.gz
tar zxvf nagios-plugins-2.1.2.tar.gz
{% endhighlight %}


### Nagios Core Installation

{% highlight bash %}
cd nagios-4.2.0
./configure  --with-nagios-group=nagios --with-command-group=nagcmd --with-httpd-conf=/etc/apache2/ --prefix=/etc/nagios

make all
make install
make install-init
make install-config
make install-commandmode
make install-webconf
{% endhighlight %}

#### Copy Nagios Core Files

{% highlight bash %}
cp -R contrib/eventhandlers/ /etc/nagios/libexec/
chown -R nagios:nagios /etc/nagios/libexec/eventhandlers
/etc/nagios/bin/nagios -v /etc/nagios/etc/nagios.cfg
{% endhighlight %}

### Setup Apache

{% highlight bash %}
mv /etc/apache2/nagios.conf /etc/apache2/sites-available/
ln -s /etc/apache2/sites-available/nagios.conf /etc/apache2/sites-enabled/
sudo a2ensite nagios
sudo a2enmod rewrite cgi
htpasswd –c /etc/nagios/etc/htpasswd.users nagiosadmin
{% endhighlight %}

### Nagios Plugins Installation

{% highlight bash %}
cd /tmp/nagios-plugins-2.1.2
./configure --with-nagios-user=nagios --with-nagios-group=nagios --prefix=/etc/nagios
make
make install
{% endhighlight %}

### Nagios Web Interface

* After correctly following the procedures you should now be able to access your Nagios Core installation from a
web browser.
* Simply use the following: <a href="http://1.1.1.1/nagios">http://1.1.1.1/nagios</a>
* Log in with the credentials you chose when adding the nagiosadmin user to the htpasswd.users file.
