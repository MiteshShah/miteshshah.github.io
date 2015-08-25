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
  url: https://cloud.githubusercontent.com/assets/1223371/9406579/5707fa08-481f-11e5-848a-d2ac7a184626.png
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
NGINX_ACCESS %{IPORHOST:visitor_ip} (?:-|(%{WORD}.%{WORD})) %{WORD:nginx_cache_status} \[%{HTTPDATE:timestamp}\] %{HOST:nginx_host} "%{WORD:method} %{URIPATHPARAM:request} HTTP/%{NUMBER:httpversion}" %{NUMBER:response} %{NUMBER:bytes} %{QS:ignore} %{QS:referrer}
NGINX_ERROR %{DATE} %{TIME} %{GREEDYDATA:error} limiting requests, excess: %{GREEDYDATA:limit} client: %{IPORHOST:visitor_ip}, server: %{HOST:nginx_host}, request: "%{WORD:method} %{URIPATHPARAM:request} HTTP/%{NUMBER:httpversion}", %{GREEDYDATA:msg}
{% endhighlight %}

{% highlight bash %}
# Create NGINX Access Log configuration file
$ vim /etc/logstash/conf.d/nginx.conf
input {
  file {
    type => "nginx"
    start_position => "beginning"
    path => [ "/var/log/nginx/*.log" ]
  }
}
filter {
  if [type] == "nginx" {
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINX_ACCESS}" }
	remove_tag => ["_grokparsefailure"]
	add_tag => ["nginx_access"]
    }
    grok {
	patterns_dir => "/etc/logstash/patterns"
	match => { "message" => "%{NGINX_ERROR}" }
	remove_tag => ["_grokparsefailure"]
	add_tag => ["nginx_error"]
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
<script src="https://gist-it.appspot.com/github/MiteshShah/ELK-Stack/blob/master/Nginx.json"></script>

### Let's Monitor NGINX/WordPress/EasyEngine Logs on Kibana

* Open http://192.168.0.1:5601/#/dashboard/NGINX?_g=()

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
