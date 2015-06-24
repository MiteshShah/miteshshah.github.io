---
layout: post
title: "How to Fix Webroot Permissions"
author:
modified:
comments: true
categories: sysadmin
excerpt: "How to reset files and directory Permissions with single command"
tags: [Linux, SysAdmin, SystemAdmin, Permissions]
image:
  feature:
date: 2015-06-23T11:11:59+05:30
---

{% include _toc.html %}

### Change Ownership
{% highlight bash %}
[mitesh@Matrix ~]$ chown -R www-data:www-data /var/www
{% endhighlight %}

**NOTE!**: The `www-data` user is used by nginx and php5-fpm.
If you are running php as a different user then change ownership as per that.
{: .notice}

### Reset Directory/File Permissions
{% highlight bash %}
# Correct Directory Permissions
[mitesh@Matrix ~]$ find /var/www -type d -exec chmod 0755 {} \;
# Correct Files Permissions
[mitesh@Matrix ~]$ find /var/www -type f -exec chmod 0644 {} \;
{% endhighlight %}
