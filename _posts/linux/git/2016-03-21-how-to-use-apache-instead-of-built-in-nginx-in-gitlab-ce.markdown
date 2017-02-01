---
layout: post
title: How to Use Apache Instead of Built-in NGINX in Gitlab CE
author:
modified:
comments: true
categories: linux/git
excerpt: "How to use Apache on Gitlab CE."
tags: [Linux, GIT, Gitlab, NGINX, Apache]
image:
  url: https://about.gitlab.com/images/press/logo/wm_no_bg.svg
  alt: How to Use Apache Instead of Built-in NGINX in Gitlab CE
  title: How to Use Apache Instead of Built-in NGINX in Gitlab CE
  feature:
date: 2016-03-21T15:25:24+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Issue

* I wanted to setup additional sites on the server using Apache, but now port 80 was already bound to by GitLab’s built-in Nginx web server.

### Stop Gitlab Services

{% highlight bash %}
$ gitlab-ctl stop
ok: down: logrotate: 0s, normally up
ok: down: postgresql: 1s, normally up
ok: down: redis: 0s, normally up
ok: down: nginx: 0s, normally up
ok: down: sidekiq: 0s, normally up
ok: down: unicorn: 0s, normally up
{% endhighlight %}

### Disable built-in NGINX

{% highlight bash %}
$ vim /etc/gitlab/gitlab.rb

# Uncomment the web server username and group settings, and set both to Apache.
web_server['username'] = 'apache'
web_server['group'] = 'apache'

# In the next section, “GitLab Nginx,” uncomment the first line and set it to false.
nginx['enable'] = false
{% endhighlight %}

### Reconfigure GitLab
{% highlight bash %}
# Avoid postgresql is not running errors.
$ gitlab-ctl start postgresql
$ gitlab-ctl reconfigure
{% endhighlight %}

### Setup Apache

{% highlight bash %}
<VirtualHost *:80>
  ServerName git.example.com
  DocumentRoot /opt/gitlab/embedded/service/gitlab-rails/public

  ProxyPreserveHost On
  AllowEncodedSlashes Off

  <Location />
    Order deny,allow
    Allow from all
    ProxyPassReverse http://127.0.0.1:8080
    ProxyPassReverse http://git.example.com/
  </Location>

  RewriteEngine on
  RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
  RewriteRule .* http://127.0.0.1:8080%{REQUEST_URI} [P,QSA]
</VirtualHost>
{% endhighlight %}

#### Restart Apache
{% highlight bash %}
$ httpd -t && sudo service httpd restart
{% endhighlight %}

### Start Gitlab Services

{% highlight bash %}
$ gitlab-ctl restart
ok: run: logrotate: (pid 22857) 0s
ok: run: postgresql: (pid 22860) 1s
ok: run: redis: (pid 22868) 0s
ok: run: sidekiq: (pid 22872) 1s
ok: run: unicorn: (pid 22876) 0s
{% endhighlight %}

**NOTE:** NGINX should not be listed anymore.
{: .notice }
