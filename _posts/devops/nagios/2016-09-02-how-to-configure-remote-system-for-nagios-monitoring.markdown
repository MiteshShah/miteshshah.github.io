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
$ ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-openssl --with-ping-command=ping
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

#### Setup NRPE Config & Plugins

{% highlight bash %}
$ mv /usr/local/nagios/etc/nrpe.cfg /usr/local/nagios/etc/nrpe.cfg.bak
$ wget -O /usr/local/nagios/etc/nrpe.cfg https://gist.githubusercontent.com/MiteshShah/77445d56396691b71ac76fb931c8ecbb/raw/3a60256314a2fb489feaecbcbb79d22b5dacddd9/nrpe.conf

$ wget -O /usr/local/nagios/libexec/check_free_mem https://gist.githubusercontent.com/MiteshShah/65dad7fa814a31d5a8a5d7bc7716c079/raw/5c2693fedfda4eda46e71b93db7c1dcf8db0f2aa/check_free_mem
$ chmod a+x /usr/local/nagios/libexec/check_free_mem

$ wget -O /usr/local/nagios/libexec/check_nginx.sh https://gist.githubusercontent.com/MiteshShah/49279e58a73ff76f34f294dfce5af92d/raw/b00f77a79b6e89eb3c085142ff665af750873fec/check_nginx.sh
$ chmod a+x /usr/local/nagios/libexec/check_nginx.sh

$ wget -O /usr/local/nagios/libexec/check_phpfpm_status.pl https://gist.githubusercontent.com/MiteshShah/c5f23b253cb439030e5db2db1c6f4eda/raw/d286c86ee38e962e28dae9f121e983d5be7216c3/check_phpfpm_status.pl
$ chmod a+x /usr/local/nagios/libexec/check_phpfpm_status.pl

$ wget -O /usr/local/nagios/libexec/check_mysqld.pl https://gist.githubusercontent.com/MiteshShah/e4ca2ddfe0e87344f94f79b8ae6895f7/raw/050857339623f33275657b5cebc4bb2f6f0587a8/check_mysqld.pl
$ chmod a+x /usr/local/nagios/libexec/check_mysqld.pl

$ service nrpe restart

{% endhighlight %}

### Enable NGINX/PHP status page

{% highlight bash %}
$ wget -O /etc/nginx/sites-available/status.conf https://gist.githubusercontent.com/MiteshShah/38591a384c426a63ec3b002ac0208801/raw/7cbf2cfafe142b1e048594ed72ed9a5b3dc31f09/status.conf
$ ln -s /etc/nginx/sites-available/status.conf /etc/nginx/sites-enabled/
$ nginx -t && service nginx restart
{% endhighlight %}
