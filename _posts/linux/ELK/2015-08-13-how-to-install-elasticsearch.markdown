---
layout: post
title: How to Install ElasticSearch
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to install ElasticSearch – The Open Source, Distributed, RESTful Search Engine"
tags: [Linux, ELK, ElasticSearch, Devops]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-13T14:25:37+05:30
---

{% include _toc.html %}

### Elastic{Search}

* Elasticsearch is a search server based on Lucene.
* It provides a distributed, multitenant-capable full-text search engine with a RESTful web interface and schema-free JSON documents.


### Install Elastic{Search} on Debian/Ubuntu

#### Download and install the Public Signing Key
{% highlight bash %}
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
{% endhighlight %}

#### Setup Repository
{% highlight bash %}
$ echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elk.list
{% endhighlight %}

#### Install Elasticsearch
{% highlight bash %}
$ sudo apt-get update && sudo apt-get install elasticsearch
{% endhighlight %}

#### Configure Elasticsearch to automatically start during bootup
{% highlight bash %}
# Debian 8
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable elasticsearch.service

# Ubuntu
$ sudo update-rc.d elasticsearch defaults 95 10
{% endhighlight %}


### Install Elastic{Search} on CentOS

#### Download and install the Public Signing Key
{% highlight bash %}
$ rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
{% endhighlight %}

#### Setup Repository
{% highlight bash %}
$ sudo vim /etc/yum.repos.d/elk.repo
[elasticsearch-2.x]
name=Elasticsearch repository for 2.x packages
baseurl=http://packages.elastic.co/elasticsearch/2.x/centos
gpgcheck=1
gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1
{% endhighlight %}

#### Install Elasticsearch
{% highlight bash %}
$ yum install elasticsearch
{% endhighlight %}

#### Configure Elasticsearch to automatically start during bootup
{% highlight bash %}
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable elasticsearch.service
{% endhighlight %}
