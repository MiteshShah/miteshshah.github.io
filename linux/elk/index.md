---
layout: page
title: "ElasticSearch Logstash Kibana"
tags: [Linux, ELK, Tutorials, ElasticSearch, Logstash, Kibana, Collectd]
modified: 2015-05-20T17:10:00.12345-05:30
comments: true
image:
  feature: https://cloud.githubusercontent.com/assets/1223371/9324389/5dc77f32-45a7-11e5-81fd-ff82a508ad97.png
  credit:
  creditlink:
---

{% include _toc.html %}
{% include _subscribe.html %}

### Requirements Overview

#### Collectd

* Collectd – The system statistics collection daemon.
* collectd gathers statistics about the system it is running on and stores this information.
* Those statistics can then be used to find current performance bottlenecks (i.e. performance analysis) and predict future system load (i.e. capacity planning)

#### Elastic{Search}

* Elasticsearch is a search server based on Lucene.
* It provides a distributed, multitenant-capable full-text search engine with a RESTful web interface and schema-free JSON documents.

#### Logstash

* Logstash is a tool for managing events and logs.
* You can use it to collect logs, parse them, and store them for later use (like, for searching).
* If you store them in Elasticsearch, you can view and analyze them with Kibana.

#### Kibana

* Kibana is an open source data visualization plugin for Elasticsearch.
* It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster.
* Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.

### Install Software
1. <a href="/linux/elk/how-to-install-collectd/"> Install Collectd </a>
1. <a href="/linux/elk/how-to-install-elasticsearch/"> Install ElasticSearch </a>
1. <a href="/linux/elk/how-to-install-logstash/"> Install Logstash </a>
1. <a href="/linux/elk/how-to-install-kibana/"> Install Kibana </a>

### Configure Software

1. <a href="/linux/elk/how-to-configure-collectd/"> Configure Collectd </a>
1. <a href="/linux/elk/how-to-configure-elasticsearch/"> Configure ElasticSearch </a>
1. <a href="/linux/elk/how-to-configure-logstash/"> Configure Logstash </a>
1. <a href="/linux/elk/how-to-configure-kibana/"> Configure Kibana </a>

### Import Squid3 Logs on ELK Stack

1. <a href="/linux/elk/how-to-monitor-squid3-logs-on-elk-stack/"> Import Squid3 Logs on ELK Stack </a>

### Import Fail2Ban Logs on ELK Stack

1. <a href="/linux/elk/how-to-monitor-fail2ban-logs-on-elk-stack/"> Import Fail2Ban Logs on ELK Stack </a>

### TODO

**NOTE!**: The following articles has been under testing and published soon.
{: .notice}


### Monitor NGINX Logs

### Monitor System Logs
