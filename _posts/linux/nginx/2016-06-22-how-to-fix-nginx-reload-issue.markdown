---
layout: post
title: How to Fix NGINX Reload Issue
author:
modified:
comments: true
categories: linux/nginx
excerpt: "How to fix - reload: Job is not running: nginx"
tags: [Linux, NGINX]
image:
  url: http://nginx.org/nginx.png
  alt: How to Fix NGINX Reload Issue
  title: How to Fix NGINX Reload Issue
  feature:
date: 2016-06-22T20:09:47+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### NGINX reload issue

{% highlight bash %}
# Check NGINX configuration
$^_^[root@MiteshShah:~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# As NGINX configuration test passed
# Let's reload NGINX
^_^[root@MiteshShah:~]# service nginx reload
reload: Job is not running: nginx
{% endhighlight %}

### How to Fix
{% highlight bash %}
# Kill all the NGINX listen port
O_O[root@MiteshShah:~]# fuser -k 80/tcp;
80/tcp:               3145  3146
^_^[root@MiteshShah:~]# fuser -k 443/tcp;
443/tcp:               3150  3151

# Check NGINX configuration
$^_^[root@MiteshShah:~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# As NGINX configuration test passed
# Let's restart NGINX
O_O[root@MiteshShah:~]# service nginx restart
nginx stop/waiting
nginx start/running, process 26068

# Now Let's reload NGINX
^_^[root@MiteshShah:~]# service nginx reload
^_^[root@MiteshShah:~]# echo $?
0
{% endhighlight %}

**NOTE!:** After killing NGINX connection & restart NGINX Next time NGINX reload works properly.
{: .notice}
