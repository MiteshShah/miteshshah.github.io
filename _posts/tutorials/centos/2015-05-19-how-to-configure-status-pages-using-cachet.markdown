---
layout: post
title: "How to Configure Status Pages Using Cachet"
modified:
categories: tutorials/centos
excerpt: "Beautiful & simple service statuses.
An open source status page system, for everyone."
tags: [Status Page, Cachet, CentOS]
image:
  feature: cachethq.jpg
  credit: cachethq.io
  creditlink: https://cachethq.io/img/main-interface.jpg
date: 2015-05-19T16:19:07+05:30
---

## Requirements:

* CentOS 7
* Knowledge of command line and shell scripts

## Setup Cachet:

{% highlight bash %}
[mitesh@status.example.com ~]$ wget -c https://goo.gl/1IjimY -O status-page.sh
[mitesh@status.example.com ~]$ sudo bash status-page.sh http://status.example.com
{% endhighlight %}

The installation may take a little time, so grab a cup of coffee <i class="fa fa-coffee"></i> and relax.

## Configure Cachet:

* Once installation done open <a href="http://status.example.com"> http://status.example.com </a>
* Now setup CachetHQ as shown in the images below.
![step1](https://cloud.githubusercontent.com/assets/1223371/7701697/5b2060de-fe47-11e4-8ce8-2978430206b2.png)
![step2](https://cloud.githubusercontent.com/assets/1223371/7701698/5b220f60-fe47-11e4-9c9e-d871fda3c506.png)
![step3](https://cloud.githubusercontent.com/assets/1223371/7701699/5b47a54a-fe47-11e4-8b88-dbe110ffa164.png)
