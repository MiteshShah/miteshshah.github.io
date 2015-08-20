---
layout: post
title: How to Configure Kibana
author:
modified:
comments:
categories: linux/ELK
excerpt: "Step by step guide to configure Kibana -  An open source data visualization plugin for Elasticsearch."
tags: [Linux, ELK, Kibana]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9324389/5dc77f32-45a7-11e5-81fd-ff82a508ad97.png
  alt:
  title:
  feature:
date: 2015-08-18T15:23:21+05:30
---

{% include _toc.html %}

### Start Kibana
{% highlight bash %}
$ /opt/kibana-4.1.1-linux-x64/bin/kibana
{% endhighlight %}

### Configure kibana4

* Open http://192.168.0.1:5601
* Click on Settings > Objects > Import

* Import Dashboard/Visualizations
<script src="https://gist.github.com/MiteshShah/eac6b7a4bfcc2e0b277e.js"></script>

### Kibana Dashboard

* Open http://192.168.0.1:5601/#/dashboard/Gateway?_g=()

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
