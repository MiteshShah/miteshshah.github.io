---
layout: post
title: Realtime Log Monitoring in Your Browser
author:
modified:
comments: true
categories: sysadmin
excerpt: "How to monitor logs realtime in your web browser."
tags: [Linux, Logs, Browser, SysAdmin]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/10360883/156761ce-6dc2-11e5-8937-c2abe800b5f3.png
  alt: Realtime Log Monitoring in Your Browser
  title: Realtime Log Monitoring in Your Browser
  feature:
date: 2015-10-08T13:38:13+05:30
---

{% include _toc.html %}

<img alt="Realtime Log Monitoring" src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Install Log.io

{% highlight bash %}
$ sudo apt-get install nodejs-legacy npm
$ sudo npm install -g log.io --user "ubuntu"
{% endhighlight %}

### Launch server
{% highlight bash %}
$ log.io-server
{% endhighlight %}

### Configure Log Harvester
{% highlight bash %}
$ cat ~/.log.io/harvester.conf
exports.config = {
  nodeName: "server01",
  logStreams: {
    examplecom: [
      "/var/log/nginx/example.com.access.log",
      "/var/log/nginx/example.com.access.log"
    ],
    miteshcom: [
      "/var/log/nginx/mitesh.com.access.log",
      "/var/log/nginx/mitesh.com.access.log"
    ]
  },
  server: {
    host: '0.0.0.0',
    port: 28777
  }
}
{% endhighlight %}

### Start log harvester
{% highlight bash %}
$ log.io-harvester
{% endhighlight %}

### Let's Monitor Logs on Browser

* Open <a href="http://localhost:28778">http://localhost:28778</a>
<img alt="Realtime Log Monitoring" src="https://cloud.githubusercontent.com/assets/1223371/10362215/de197fe6-6dca-11e5-8f60-b0c1ce0e06ce.png">

### More Information

* <a href="http://logio.org:28778/">Try Demo</a>
* <a href="http://logio.org/">http://logio.org</a>
