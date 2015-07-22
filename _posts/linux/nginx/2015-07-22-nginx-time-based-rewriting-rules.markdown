---
layout: post
title: "NGINX - Time Based Rewriting Rules"
author:
modified:
comments: true
categories: linux/nginx
excerpt: "How to tell NGINX to change it's behavior for specific time windows. Extremely useful for lazy system admins like me"
tags: [Linux, NGINX, Rewrite Rules, HttpLuaModule]
image:
  url:
  alt:
  title:
  feature:
date: 2015-07-22T18:20:41+05:30
---

* Last night one of my friend asked me to write some simple re-write rules for his website maintenance.
* So all the visitors should be rewrite to the maintenance page easy right ?
* Unfortunately, the website maintenance windows is between 2:00 AM to 4:00 AM, and i was not interested on stay awake till that time.


{% highlight lua %}

location / {
  rewrite_by_lua '
    local current_time = os.time()
    local start_time = os.time({year=2015,month=7,day=22,hour=02,min=0,sec=0})
    local end_time = os.time({year=2015,month=7,day=22,hour=04,min=00,sec=0})

    if current_time >= start_time and current_time <= end_time then
      return ngx.redirect("https://miteshshah.github.io/maintenance.html")
    end
  ';
}
{% endhighlight %}
