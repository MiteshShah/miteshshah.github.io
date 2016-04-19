---
layout: post
title: "Auto Reload NGINX"
author:
modified:
comments: true
categories: linux/nginx
excerpt: "How to automatic reload NGINX, When some of website configuration files Modify/Created/Deleted."
tags: [NGINX, ShellScripting]
image:
  feature:
date: 2015-06-24T18:10:10+05:30
---

{% include _toc.html %}

### Requirements

* inotify-tools
* nginx

### Auto Reload NGINX

#### NGINX Virtual Host
{% highlight bash %}
[mitesh@Matrix ~]$  cat /etc/nginx/nginx.conf
##
# Virtual Host Configs
##

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
{% endhighlight %}

* As you noticed all the Virtual Host Configuration is stored at `/etc/nginx/sites-enabled/` location.
* The `/etc/nginx/sites-enabled/` location contains only symbolic link of `/etc/nginx/sites-available/`.

**For Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l /etc/nginx/sites-available/example.com
-rw-r--r-- 1 root root 418 Jun 23 18:14 /etc/nginx/sites-available/example.com
[mitesh@Matrix ~]$ ls -l /etc/nginx/sites-enabled/example.com
lrwxrwxrwx 1 root root 38 Jun 23 18:14 /etc/nginx/sites-enabled/example.com -> /etc/nginx/sites-available/example.com
{% endhighlight %}

#### Shell Script To Auto Reload NGINX
{% highlight bash %}
#!/bin/bash
# Check inotify-tools is installed or not
dpkg --get-selections | grep -v deinstall | grep inotify-tools &> /dev/null
if [ $? -ne 0 ]
then
        echo "Installing inotify-tools, please wait..."
        apt-get -y install inotify-tools
fi
while true
do
        inotifywait --exclude .swp -e create -e modify -e delete -e move  /etc/nginx/sites-enabled
        # Check NGINX Configuration Test
        # Only Reload NGINX If NGINX Configuration Test Pass
        nginx -t
        if [ $? -eq 0 ]
        then
                echo "Reloading Nginx Configuration"
                service nginx reload
        fi
done
{% endhighlight %}
