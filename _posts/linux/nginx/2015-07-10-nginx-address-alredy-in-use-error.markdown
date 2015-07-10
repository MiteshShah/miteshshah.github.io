---
layout: post
title: "NGINX - Address Alredy in Use Error"
author:
modified:
comments: true
categories: linux/nginx
excerpt: "How to fix NGINX Error - [emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)"
tags: [NGINX]
image:
  feature:
date: 2015-07-10T13:38:04+05:30
---

{% include _toc.html %}

### Pre checks

#### Port 80
* Check No Other Service Is Running on Port 80
{% highlight bash %}
$ sudo netstat -tluap --numeric-ports | grep '80 '
{% endhighlight %}

#### Port 443
* Check No Other Service Is Running on Port 443
{% highlight bash %}
$ sudo netstat -tluap --numeric-ports | grep '443'
{% endhighlight %}


**NOTE!**: If you found Apache or Other Service is Running on Port 80/443, Then Stop That Service and Restart NGINX.
{: .notice}

### How to Fix NGINX - Address Alredy in Use Error

#### Port 80
* If no other service is running on port 80, Then execute Following Command.

{% highlight bash %}
$ sudo fuser -k 80/tcp;
$ sudo service nginx restart
{% endhighlight %}

#### Port 443
* If no other service is running on port 443, Then execute Following Command.

{% highlight bash %}
$ sudo fuser -k 443/tcp;
$ sudo service nginx restart
{% endhighlight %}
