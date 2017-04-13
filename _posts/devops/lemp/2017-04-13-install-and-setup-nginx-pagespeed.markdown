---
layout: post
title: Install & Setup NGINX PageSpeed
author:
modified:
comments: true
categories: devops/lemp
excerpt: "Step By Step guide to install & setup NGINX with PageSpeed Module"
tags: [SysAdmin, Devops, LEMP, NGINX, Ubuntu, Debian]
image:
  url:
  alt: Install & Setup NGINX
  title: Install & Setup NGINX
  feature:
date: 2017-04-13T12:20:32+05:30
---


{% include _toc.html %}

#### Install NGINX Pagespeed

{% highlight bash %}
$ sudo add-apt-repository ppa:ansipress/nginx
$ sudo apt-get update
$ sudo apt-get install nginx-pagespeed
{% endhighlight %}

#### Configure NGINX

{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/acl.conf
##
# ACL Settings
##

# HTTP authentication || IP address
satisfy any;
auth_basic "Restricted Area";
auth_basic_user_file htpasswd;

# Allowed IP Address List
allow 127.0.0.1;
deny all;
{% endhighlight %}

{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/expires.conf
##
# Cache Static Files
##

# Feed
location ~* \.(rss|atom)$ {
  expires 1h;
}

# Media: images, icons, video, audio, htc
location ~* \.(jpg|jpeg|gif|png|ico|cur|bmp|svg|svgz|mp4|ogg|ogv|webm|mid|midi|wav|htc|swf)$ {
  expires max;
  access_log off;
  log_not_found off;
  add_header Cache-Control "public";
}

# CSS and Javascript
location ~* \.(css|js)$ {
  expires max;
  access_log off;
  log_not_found off;
}

# WebFonts
location ~* \.(ttf|ttc|otf|eot|woff|woff2)$ {
  expires 1M;
  access_log off;
  log_not_found off;
  add_header Cache-Control "public";
  add_header "Access-Control-Allow-Origin" "*";
}

location ~* \.(zip|gz|tar|tgz|rar|bz2|exe|doc|xls|ppt|rtf)$ {
  expires max;
  access_log off;
  log_not_found off;
}
{% endhighlight %}

{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/locations.conf
##
# Basic Locations Files
##

location = /robots.txt {
  try_files $uri $uri/ /index.php?$args;
  access_log off;
  log_not_found off;
}
{% endhighlight %}

{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/protect-system-files.conf
##
# Protect System Files
##

# https://www.mnot.net/blog/2010/04/07/well-known
location ~ /\.well-known {
 allow all;
}

# Deny hidden files
location ~ /\. {
 deny all;
 access_log off;
 log_not_found off;
}

# Deny backup extensions & log files
location ~* ^.+\.(txt|bak|log|old|orig|original|php#|php~|php_bak|save|sql|conf|dist|fla|psd|sh|in[ci]|sw[op])$ {
 deny all;
 access_log off;
 log_not_found off;
}

# Return 403 forbidden for readme.(txt|html) or license.(txt|html) or example.(txt|html)
if ($uri ~* "^.+(readme|license|example)\.(txt|html)$") {
 return 403;
}
{% endhighlight %}


{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/status.conf
##
# Status Pages
##

location /nginx_status {
  stub_status on;
  access_log off;
  include ansipress/acl.conf;
}
{% endhighlight %}


{% highlight bash %}
$ sudo vim /etc/nginx/ansipress/pagespeed.conf
##
# Google PageSpeed Settings
##

# PageSpeed Admin
location /ngx_pagespeed_statistics { include ansipress/acl.conf; }
location /ngx_pagespeed_global_statistics { include ansipress/acl.conf; }
location /ngx_pagespeed_message { include ansipress/acl.conf; }
location /pagespeed_console { include ansipress/acl.conf; }
location ~ ^/pagespeed_admin { include ansipress/acl.conf; }
location ~ ^/pagespeed_global_admin { include ansipress/acl.conf; }

# This is a temporary workaround that ensures requests for pagespeed
# optimized resources go to the pagespeed handler.
location ~ ".pagespeed.([a-z].)?[a-z]{2}.[^.]{10}.[^.]+" { }
location ~ "^/ngx_pagespeed_static/" { }
location ~ "^/ngx_pagespeed_beacon$" { }
{% endhighlight %}


{% highlight bash %}
$ sudo vim /etc/nginx/conf.d/pagespeed.conf
##
# Google PageSpeed Settings
##

# Turning the module on and off
pagespeed on;

# Configuring PageSpeed Filters
pagespeed RewriteLevel PassThrough;

# Needs to exist and be writable by nginx.
# Use tmpfs for best performance.
pagespeed MemcachedThreads 1;
pagespeed MemcachedServers "127.0.0.1:11211";
pagespeed FileCachePath /run/ngx_pagespeed_cache;

# PageSpeed Admin
pagespeed StatisticsPath /ngx_pagespeed_statistics;
pagespeed GlobalStatisticsPath /ngx_pagespeed_global_statistics;
pagespeed MessagesPath /ngx_pagespeed_message;
pagespeed ConsolePath /pagespeed_console;
pagespeed AdminPath /pagespeed_admin;
pagespeed GlobalAdminPath /pagespeed_global_admin;

# PageSpeed Cache Purge
pagespeed EnableCachePurge on;
pagespeed PurgeMethod PURGE;
{% endhighlight %}

#### Setup HTTP AUTH

{% highlight bash %}
$ sudo sh -c "echo -n 'mitesh:' >> /etc/nginx/htpasswd"
$ sudo sh -c "openssl passwd -apr1 >> /etc/nginx/htpasswd"
{% endhighlight %}
