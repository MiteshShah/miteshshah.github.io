---
layout: post
title: How to Install Logstash
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to install Logstash - Transport and process your logs, events, or other data"
tags: [Linux, ELK, Logstash]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-13T14:25:47+05:30
---

{% include _toc.html %}

### Logstash

* Logstash is a tool for managing events and logs.
* You can use it to collect logs, parse them, and store them for later use (like, for searching).
* If you store them in Elasticsearch, you can view and analyze them with Kibana.

### Install Logstash on Debian/Ubuntu

#### Download and install the Public Signing Key
{% highlight bash %}
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
{% endhighlight %}

#### Setup Repository
{% highlight bash %}
$ echo "deb http://packages.elasticsearch.org/logstash/1.5/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elk.list
{% endhighlight %}

#### Install Logstash
{% highlight bash %}
$ sudo apt-get update && sudo apt-get install logstash
{% endhighlight %}

#### Configure Logstash to automatically start during bootup
{% highlight bash %}
# Debian 8
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable logstash.service

# Ubuntu
$ sudo update-rc.d logstash defaults 95 10
{% endhighlight %}


### Install Logstash on CentOS

#### Download and install the Public Signing Key
{% highlight bash %}
$ rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
{% endhighlight %}

#### Setup Repository
{% highlight bash %}
$ sudo vim /etc/yum.repos.d/elk.repo
[logstash-1.5]
name=Logstash repository for 1.5.x packages
baseurl=http://packages.elastic.co/logstash/1.5/centos
gpgcheck=1
gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1
{% endhighlight %}

#### Install Logstash
{% highlight bash %}
$ yum install logstash
{% endhighlight %}

#### Configure Logstash to automatically start during bootup
{% highlight bash %}
$ sudo /sbin/chkconfig logstash on
{% endhighlight %}
