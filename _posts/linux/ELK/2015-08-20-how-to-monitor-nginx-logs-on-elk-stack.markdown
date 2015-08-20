---
layout: post
title: How to Monitor NGINX Logs on ELK Stack
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to configure NGINX/WordPress/EasyEngine Logs on ELK Stack."
tags: [Linux, ELK, NGINX, EasyEngine, WordPress]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9383866/c9519a76-4769-11e5-9a38-78d00e4869f7.png
  alt: NGINX WordPress EasyEngine Logs on ELK Stack
  title: NGINX WordPress EasyEngine Logs on ELK Stack
  feature:
date: 2015-08-20T18:21:24+05:30
---
{% include _toc.html %}


### Import NGINX/WordPress/EasyEngine(ee) Logs on Logstash

**NOTE!** We assume that you already setup/configure <a href="/linux/elk">ELK Stack</a>.
{: .notice}

* To Import NGINX Logs on Logstash, We have to create configuration file.

{% highlight bash %}
# Create NGINX/EasyEngine Patterns
# http://grokdebug.herokuapp.com/
$ mkdir /etc/logstash/patterns
$ cat /etc/logstash/patterns/nginx
NGUSERNAME [a-zA-Z\.\@\-\+_%]+
NGUSER %{NGUSERNAME}
NGINXACCESS %{IPORHOST:visitor_ip} (?:-|(%{WORD}.%{WORD})) %{WORD:nginx_cache_status} \[%{HTTPDATE:timestamp}\] %{HOST:nginx_host} "%{WORD:method} %{URIPATHPARAM:request} HTTP/%{NUMBER:httpversion}" %{NUMBER:response} %{NUMBER:bytes} %{QS:ignore} %{QS:referrer}
NGINXERROR %{DATE} %{TIME} %{GREEDYDATA:msg} limiting requests, excess: %{GREEDYDATA:limit} client: %{IPORHOST:visitor_ip}, server: %{HOST:nginx_host}, request: "%{WORD:method} %{URIPATHPARAM:request} HTTP/%{NUMBER:httpversion}", %{GREEDYDATA:msg}

# Create Logstash configuration file
$ vim /etc/logstash/conf.d/nginx.conf
input {
  file {
    type => "nginxaccess"
    start_position => "beginning"
    path => [ "/var/log/nginx/*.access.log" ]
  }
}
filter {
  if [type] == "nginxaccess" {
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINXACCESS}" }
    }
    geoip {
      source => "visitor_ip"
    }
  }
}

input {
  file {
    type => "nginxerror"
    start_position => "beginning"
    path => [ "/var/log/nginx/*.error.log" ]
  }
}
filter {
  if [type] == "nginxerror" {
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINXERROR}" }
    }
    geoip {
      source => "visitor_ip"
    }
  }
}
{% endhighlight %}

### Fix NGINX Logs Permission

* Let's make Fail2Ban logs are readable by Logstash

{% highlight bash %}
# Temp Fix
$ chmod 644 /var/log/*.log

# Permeant Fix
$ cat /etc/logrotate.d/nginx
/var/log/nginx/*.log {
	size 10M
	missingok
	rotate 52
	compress
	delaycompress
	notifempty
	create 0644 www-data adm
	sharedscripts
	prerotate
		if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
			run-parts /etc/logrotate.d/httpd-prerotate; \
		fi \
	endscript
	postrotate
		invoke-rc.d nginx rotate >/dev/null 2>&1
	endscript
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
<script src="https://gist.github.com/MiteshShah/23bde5446c34751bcf0f.js"></script>

### Let's Monitor NGINX/WordPress/EasyEngine Logs on Kibana

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
