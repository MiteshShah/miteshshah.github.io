---
layout: post
title: How to Install Kibana
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to install Kibana -  An open source data visualization plugin for Elasticsearch."
tags: [Linux, ELK, Kibana]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-13T14:25:53+05:30
---

{% include _toc.html %}

### Kibana

* Kibana is an open source data visualization plugin for Elasticsearch.
* It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster.
* Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.

### Install Kibana

{% highlight bash %}
$ cd /opt
$ sudo wget -c https://download.elastic.co/kibana/kibana/kibana-4.3.0-linux-x64.tar.gz -O /opt/kibana.tar.gz
$ tar -zxvf kibana.tar.gz && rm kibana.tar.gz
{% endhighlight %}
