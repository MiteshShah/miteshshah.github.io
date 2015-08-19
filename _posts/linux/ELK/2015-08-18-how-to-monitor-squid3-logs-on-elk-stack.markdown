---
layout: post
title: How to Monitor Squid3 Logs on ELK Stack
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to configure Squid3 Logs on ELK Stack."
tags: [Linux, ELK, Squid3]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9349332/494af7bc-4662-11e5-8f14-b228b92cbb2e.png
  alt: How to Monitor Squid3 Logs on ELK Stack
  title: How to Monitor Squid3 Logs on ELK Stack
  feature:
date: 2015-08-18T18:21:34+05:30
---

{% include _toc.html %}

### Import Squid3 Logs on Logstash

**NOTE!** We assume that you already setup/configure <a href="/linux/elk">ELK Stack</a>.
{: .notice}


* To Import squid3 Logs on Logstash, We have to create configuration file.

{% highlight bash %}
$ vim /etc/logstash/conf.d/squid.conf
input {
  file {
    type => "squid"
    start_position => "beginning"
    path => [ "/var/log/squid3/access.log" ]
  }
}

filter {
  if [type] == "squid" {
    grok {
      match => [ "message", "%{POSINT:timestamp}.%{POSINT:timestamp_ms}\s+%{NUMBER:response_time} %{IPORHOST:src_ip} %{WORD:squid_request_status}/%{NUMBER:http_status_code} %{NUMBER:reply_size_include_header} %{WORD:http_method} %{NOTSPACE:request_url} %{NOTSPACE:user} %{WORD:squid}/%{IP:dst_ip} %{NOTSPACE:content_type}" ]
      add_tag => ["squid"]
    }
    geoip {
      source => "dst_ip"
    }
  }
}
{% endhighlight %}

### Fix Squid3 Logs Permission

* Let's make squid3 logs are readable by Logstash

{% highlight bash %}
# Temp Fix
$ chmod 644 /var/log/squid3/access.log

# Permeant Fix
$ cat /etc/logrotate.d/squid3
#
#	Logrotate fragment for squid3.
#
/var/log/squid3/*.log {
	daily
	create 0644 proxy proxy
	compress
	delaycompress
	rotate 2
	missingok
	nocreate
	sharedscripts
	postrotate
		test ! -e /var/run/squid3.pid || /usr/sbin/squid3 -k rotate
	endscript
}
{% endhighlight %}

### Restart Logstash Service

{% highlight bash %}
$ sudo service logstash restart
{% endhighlight %}

### Let's Monitor Squid Logs on Kibana

* Open http://192.168.0.1:5601/

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
