---
layout: post
title: How to Monitor System With ELK Stack
author:
modified:
comments:
categories: linux/ELK
excerpt: "Step by step guide to Monitor System with ELK Stack."
tags: [Linux, ELK, Collectd, Devops]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9403774/7e805fe8-4807-11e5-8fde-4760d1a1f055.png
  alt:
  title:
  feature:
date: 2015-08-18T16:13:13+05:30
---

{% include _toc.html %}

### Monitor System With Collectd/ELK Stack

**NOTE!** We assume that you already setup/configure <a href="/linux/elk">ELK Stack</a>.
{: .notice}

### Import Collectd Logs on Logstash

* To Import Collectd Logs on Logstash, We have to create configuration file.

{% highlight bash %}
$ vim /etc/logstash/conf.d/collectd.conf
input {
  udp {
    port => 25826         # 25826 matches port specified in collectd.conf
    buffer_size => 1452   # 1452 is the default buffer size for Collectd
    codec => collectd { } # specific Collectd codec to invoke
    type => collectd
  }
}
{% endhighlight %}

### Restart Logstash Service

{% highlight bash %}
$ sudo service logstash restart
{% endhighlight %}

### Configure Kibana
* Open http://192.168.0.1:5601/
* Click on Settings > Objects > Import

* Import Dashboard/Visualizations
<script src="https://gist-it.appspot.com/github/MiteshShah/ELK-Stack/blob/master/Collectd.json"></script>


### Kibana Dashboard

* Open http://192.168.0.1:5601/#/dashboard/Gateway?_g=()

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
