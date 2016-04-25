---
layout: post
title: How to Configure ElasticSearch
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to configure ElasticSearch – The Open Source, Distributed, RESTful Search Engine"
tags: [Linux, ELK, ElasticSearch, Devops]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-18T14:27:38+05:30
---

{% include _toc.html %}

### Configure Elastic{Search}

* For ElasticSearch, We have to update elasticsearch configuration file.

{% highlight bash %}
$ cat /etc/elasticsearch/elasticsearch.yml
# Cluster name identifies your cluster for auto-discovery. If you're running
# multiple clusters on the same network, make sure you're using unique names.
cluster.name: Gateway

# Note, that for development on a local machine, with small indices, it usually
# makes sense to "disable" the distributed features:
index.number_of_shards: 1
index.number_of_replicas: 0

# Allow to create logstash index
action.auto_create_index: logstash-*
{% endhighlight %}


### Restart ElasticSearch Service

{% highlight bash %}
$ sudo service elasticsearch restart
{% endhighlight %}

### Check ElasticSearch Status

{% highlight bash %}
$ curl 127.0.0.1:9200
{
  "status" : 200,
  "name" : "Spellbinder",
  "cluster_name" : "Gateway",
  "version" : {
    "number" : "1.7.1",
    "build_hash" : "b88f43fc40b0bcd7f173a1f9ee2e97816de80b19",
    "build_timestamp" : "2015-07-29T09:54:16Z",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}
{% endhighlight %}
