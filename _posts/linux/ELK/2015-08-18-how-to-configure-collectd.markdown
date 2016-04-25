---
layout: post
title: How to Configure Collectd
author:
modified:
comments: true
categories: linux/ELK
excerpt: "Step by step guide to configure Collectd – The system statistics collection daemon"
tags: [Linux, ELK, Collectd, Devops]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-18T14:27:26+05:30
---
{% include _toc.html %}

### Configure Collectd

* For Collectd, We have to create a configuration file.

{% highlight bash %}
$ cat /etc/collectd/collectd.conf

# For each instance where collectd is running, we define
# hostname proper to that instance. When metrics from
# multiple instances are aggregated, hostname will tell
# us were they came from.
Hostname "Gateway"

# Fully qualified domain name
FQDNLookup false

# Plugins we are going to use with their configurations,
# if needed

LoadPlugin cpu
LoadPlugin df
LoadPlugin disk
LoadPlugin irq
LoadPlugin load
LoadPlugin users
LoadPlugin swap
LoadPlugin memory
LoadPlugin uptime
LoadPlugin processes
LoadPlugin ethstat
# LoadPlugin entropy
# LoadPlugin rrdtool
# LoadPlugin battery

LoadPlugin syslog
<Plugin syslog>
  LogLevel info
</Plugin>

LoadPlugin interface
<Plugin interface>
  Interface "eth0"
  IgnoreSelected false
</Plugin>

LoadPlugin network
<Plugin network>
  Server "127.0.0.1" "25826"
</Plugin>

LoadPlugin ping
<Plugin "ping">
  Host "google.co.in"
</Plugin>

<Include "/etc/collectd/collectd.conf.d">
        Filter ".conf"
</Include>
{% endhighlight %}


### Restart Collectd Service

{% highlight bash %}
$ sudo service collectd restart
{% endhighlight %}

### More Details

* Each plugin will gather different pieces of information.
* For an extensive list of plugins and their details, go to the <a href="https://collectd.org/wiki/index.php/Table_of_Plugins">Collectd Plugins</a> page.
