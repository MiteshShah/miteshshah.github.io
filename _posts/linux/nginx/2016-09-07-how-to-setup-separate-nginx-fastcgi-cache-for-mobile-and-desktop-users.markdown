---
layout: post
title: How to Setup Separate NGINX FastCGI Cache for Mobile & Desktop Users
author:
modified:
comments: true
categories: linux/nginx
excerpt: "In this guide I'll show you how to setup separate NGINX FastCGI cache for Mobile & Desktop users."
tags: [Linux, NGINX, EasyEngine]
image:
  url:
  alt: How to Setup Separate NGINX FastCGI Cache for Mobile & Desktop Users
  title: How to Setup Separate NGINX FastCGI Cache for Mobile & Desktop Users
  feature:
date: 2016-09-07T12:54:04+05:30
refer: http://community.rtcamp.com/t/how-to-fastcgi-cache-desktop-with-mobile-versions-purging-with-get-requests/6132
---


{% include _toc.html %}

### Prerequisites

* Website created with EasyEngine FastCGI cache

### Detect Mobile Request

{% highlight bash %}
$ vim map $http_user_agent $mobile_request {
 default                  fullversion;

 "~*ipad"    mobileversion;
 "~*android.*mobile"   mobileversion;
 "~*iphone"    mobileversion;
 "~*ipod.*mobile"   mobileversion;
 "~*BlackBerry*Mobile Safari"  mobileversion;
 "~*BB*Mobile Safari"   mobileversion;
 "~*Opera.*Mini/7"   mobileversion;
 "~*IEMobile/10.*Touch"   mobileversion;
 "~*IEMobile/11.*Touch"   mobileversion;
 "~*IEMobile/7.0"   mobileversion;
 "~*IEMobile/9.0"   mobileversion;
 "~*Firefox.*Mobile"   mobileversion;
 "~*webOS"    mobileversion;
}
{% endhighlight %}

### Configure NGINX FastCGI Cache

{% highlight bash %}
$ vim /etc/nginx/conf.d/fastcgi.conf
# FastCGI cache settings
fastcgi_cache_path /var/run/nginx-cache levels=1:2 keys_zone=WORDPRESS:50m inactive=60m;
#fastcgi_cache_key "$scheme$request_method$host$request_uri";
fastcgi_cache_key "$scheme$request_method$host$request_uri$mobile_request";
fastcgi_cache_use_stale error timeout invalid_header updating http_500 http_503;
fastcgi_cache_valid 200 301 302 404 1h;
fastcgi_buffers 16 16k;
fastcgi_buffer_size 32k;
fastcgi_param SERVER_NAME $http_host;
fastcgi_ignore_headers Cache-Control Expires Set-Cookie;
fastcgi_keep_conn on;
{% endhighlight %}

### Edit EE WPFC File

{% highlight bash %}
$ cp -av cp -av /etc/nginx/common/wpfc.conf /etc/nginx/common/wpfc-custom.conf
$ vim /etc/nginx/common/wpfc-custom.conf
# WPFC NGINX CONFIGURATION
# DO NOT MODIFY, ALL CHANGES LOST AFTER UPDATE EasyEngine (ee)
set $skip_cache 0;
set $var_desktop "fullversion";
set $var_mobile "mobileversion";
# POST requests and URL with a query string should always go to php
if ($request_method = POST) {
  set $skip_cache 1;
}
if ($query_string != "") {
  set $skip_cache 1;
}
# Don't cache URL containing the following segments
if ($request_uri ~* "(/wp-admin/|/xmlrpc.php|wp-.*.php|index.php|/feed/|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
  set $skip_cache 1;
}
# Don't use the cache for logged in users or recent commenter
if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {
  set $skip_cache 1;
}
# Use cached or actual file if they exists, Otherwise pass request to WordPress
location / {
  try_files $uri $uri/ /index.php?$args;
}
location ~ ^/wp-content/cache/minify/(.+\.(css|js))$ {
  try_files $uri /wp-content/plugins/w3-total-cache/pub/minify.php?file=$1;
}
location ~ \.php$ {
  try_files $uri =404;
  include fastcgi_params;
  fastcgi_pass php;
  fastcgi_cache_bypass $skip_cache;
  fastcgi_no_cache $skip_cache;
  fastcgi_cache WORDPRESS;
}
location ~ /purge(/.*) {
  fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1$var_desktop";
  access_log off;
}
location ~ /mpurge(/.*) {
  fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1$var_mobile";
  access_log off;
}
{% endhighlight %}

### Edit Website
{% highlight bash %}
$ ee site edit example.com
# replace wpfc.conf with wpfc-custom.conf
include common/wpfc-custom.conf;
{% endhighlight %}


### Purge Cache

* Desktop Cache Purge: http://example.com/purge/
* Mobile Cache Purge: http://example.com/mpurge/
