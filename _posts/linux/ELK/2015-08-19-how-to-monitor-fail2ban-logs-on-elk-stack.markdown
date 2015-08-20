---
layout: post
title: How to Monitor Fail2Ban Logs on ELK Stack
author:
modified:
comments:
categories: linux/ELK
excerpt: "Step by step guide to configure Fail2Ban Logs on ELK Stack."
tags: [Linux, ELK, Fail2Ban]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9351897/9d3674e2-467a-11e5-8513-1d4cff24210e.png
  alt: How to Monitor Fail2Ban Logs on ELK Stack
  title: How to Monitor Fail2Ban Logs on ELK Stack
  feature:
date: 2015-08-19T13:35:52+05:30
---

{% include _toc.html %}


### Import Fail2Ban Logs on Logstash

**NOTE!** We assume that you already setup/configure <a href="/linux/elk">ELK Stack</a>.
{: .notice}

* To Import Fail2Ban Logs on Logstash, We have to create configuration file.

{% highlight bash %}
# Create Fail2Ban Patterns
$ mkdir /etc/logstash/patterns
$ cat /etc/logstash/patterns/fail2ban
F2B_DATE %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[ ]%{HOUR}:?%{MINUTE}(?::?%{SECOND})
F2B_ACTION (\w+)\.(?:\w+)(\s+)?\:
F2B_JAIL \[(?<jail>\w+\-?\w+?)\]
F2B_LEVEL (?<level>\w+)\s+

# Create Logstash configuration file
$ vim /etc/logstash/conf.d/fail2ban.conf
input {
  file {
    type => "fail2ban"
    start_position => "beginning"
    path => [ "/var/log/fail2ban.log" ]
  }
}

filter {
  if [type] == "fail2ban" {
    grok {
      patterns_dir => "/etc/logstash/patterns"
      match => [
        "message", "%{F2B_DATE:date} %{F2B_ACTION} %{WORD:level} %{F2B_JAIL} %{WORD:action} %{IP:ip}",
        "message", "%{F2B_DATE:date} %{F2B_ACTION} %{F2B_LEVEL} %{GREEDYDATA:msg}?"
      ]
    }

    geoip {
      source => "ip"
    }
  }
}
{% endhighlight %}

### Fix Fail2Ban Logs Permission

* Let's make Fail2Ban logs are readable by Logstash

{% highlight bash %}
# Temp Fix
$ chmod 644 /var/log/fail2ban.log

# Permeant Fix
$ cat /etc/logrotate.d/Fail2Ban
/var/log/fail2ban.log {

    weekly
    rotate 4
    compress

    delaycompress
    missingok
    postrotate
	   fail2ban-client set logtarget /var/log/fail2ban.log >/dev/null
    endscript

    # If fail2ban runs as non-root it still needs to have write access
    # to logfiles.
    # create 644 fail2ban adm
    create 644 root adm
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
<script src="https://gist.github.com/MiteshShah/e2d1152e82455ff0d861.js"></script>

### Let's Monitor Fail2Ban Logs on Kibana

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
