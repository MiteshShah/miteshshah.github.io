---
layout: post
title: How to Configure Logstash
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to configure Logstash - Transport and process your logs, events, or other data"
tags: [Linux, ELK, Logstash]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-18T14:27:53+05:30
---

{% include _toc.html %}

### Configure Logstash

* For Logstash, We have to create/update configuration file.

{% highlight bash %}
$ cat /etc/logstash/conf.d/logstash.conf
input {
  udp {
    port => 25826         # 25826 matches port specified in collectd.conf
    buffer_size => 1452   # 1452 is the default buffer size for Collectd
    codec => collectd { } # specific Collectd codec to invoke
    type => collectd
  }
}
output {
  elasticsearch {
    cluster  => Gateway # this matches out elasticsearch cluster.name
    protocol => http
  }
}
{% endhighlight %}


### Restart Logstash Service

{% highlight bash %}
$ sudo service logstash restart
{% endhighlight %}
