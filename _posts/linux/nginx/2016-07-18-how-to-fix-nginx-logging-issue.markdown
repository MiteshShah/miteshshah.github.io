---
layout: post
title: How to Fix NGINX Logging Issue
author:
modified:
comments: true
categories: linux/nginx
excerpt: "How to fix NGINX writes logs into access.log.1 instead of access.log issue"
tags: [Linux, NGINX]
image:
  url: http://nginx.org/nginx.png
  alt: How to Fix NGINX Logging Issue
  title: How to Fix NGINX Logging Issue
  feature:
date: 2016-07-18T20:55:19+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### NGINX Logging Issue
* After looking into NGINX DEBUG & other I'd found nothing related to this issue.
* So I'd decided to run the logrotate command manually and see what's going on.

{% highlight bash %}
$ cat /etc/logrotate.d/nginx
/var/log/nginx/*.log {
	daily
	missingok
	rotate 14
	compress
	delaycompress
	notifempty
	create 0640 www-data adm
	sharedscripts
	prerotate
		if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
			run-parts /etc/logrotate.d/httpd-prerotate; \
		fi \
	endscript
	postrotate
		invoke-rc.d nginx rotate >/dev/null 2>&1
	endscript
}
{% endhighlight %}

* When I run the post rotate script manually it's through errors.

{% highlight bash %}
^_^[root@mitesh.com:~]# invoke-rc.d nginx rotate
initctl: invalid command: rotate
Try `initctl --help' for more information.
invoke-rc.d: initscript nginx, action "rotate" failed.
{% endhighlight %}

* That clearly means we have to look into post rotate script.

### Fix NGINX Logging Issue
* After doing some research and search on IRC channel,
* I'd found the solution which fix this issue.

{% highlight bash %}
^_^[root@mitesh.com:~]# service nginx rotate
 * Re-opening nginx log files nginx
{% endhighlight %}

### Fix All Servers using Ansible

* I hate manually run same command on nearly hundred of servers.
* Ansible is a good way to automate this boring work.

{% highlight bash %}
$ ansible ALL -m shell -a "sudo sed -i 's/invoke-rc.d nginx rotate/service nginx rotate/' /etc/logrotate.d/nginx"
$ ansible ALL -m shell -a "sudo service nginx rotate"
{% endhighlight %}

**NOTE!**: If you are not sure what is Ansible then check out some <a href="/devops/ansible/">Ansible Tutorials</a>.
{: .notice}
