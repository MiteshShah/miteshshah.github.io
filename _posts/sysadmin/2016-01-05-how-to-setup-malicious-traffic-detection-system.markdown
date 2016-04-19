---
layout: post
title: How to Setup Malicious Traffic Detection System
author:
modified:
comments: true
categories: sysadmin
excerpt: "Maltrail is a malicious traffic detection system, utilizing publicly available (black)lists containing malicious and/or generally suspicious trails, along with static trails compiled from various AV reports and custom user defined lists, where trail can be anything from domain name, URL."
tags: [SysAdmin, Malicious, Network, Maltrail]
image:
  url: https://camo.githubusercontent.com/2b55895a420eba31cf7806c0420b320a3c27b0df/687474703a2f2f692e696d6775722e636f6d2f7135377a7136592e706e67
  alt: How to Setup Malicious Traffic Detection System
  title: How to Setup Malicious Traffic Detection System
  feature:
date: 2016-01-05T17:50:29+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

#### Install Maltrail

{% highlight bash %}
$ sudo apt-get install python-pcapy schedtool
$ git clone https://github.com/stamparm/maltrail.git
$ cd maltrail

# Start Sensor
$ sudo python sensor.py

# Start Server http://localhost:8338
$ sudo python server.py

{% endhighlight %}

##### Default Credentials

{% highlight text %}
username: admin
password: changeme!
{% endhighlight %}

##### More Information

<a href="https://github.com/stamparm/maltrail/blob/master/README.md">Click Here</a> to access more detailed information.
