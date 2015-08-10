---
layout: post
title: NGINX/Apache Redirect Rules
author:
modified:
comments: true
categories: linux/nginx
excerpt: "All about NGINX and APACHE 301 Moved Permanently."
tags: [NGINX, APACHE, HTTPD, HTACCESS]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9169847/9c4574b2-3f7b-11e5-81ec-5ede06b0aa97.jpg
  alt: NGINX/Apache Redirect Rules
  title: NGINX/Apache Redirect Rules
  feature:
date: 2015-08-10T16:17:57+05:30
---
{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">


### How to redirect example.in to example.com

#### NGINX

{% highlight bash %}
server {
  listen 80;
  server_name example.in;
  return 301 http://example.com$request_uri;
}
{% endhighlight %}

#### Apache
{% highlight bash %}
RewriteCond %{HTTP_HOST} ^example.in [NC]
RewriteRule ^(.*)$ http://example.com/$1 [L,R=301,NC]
{% endhighlight %}

### How to redirect HTTP to HTTPS

#### NGINX

{% highlight bash %}
server {
  listen 80;
  server_name example.com;
  return 301 https://example.com$request_uri;
}
{% endhighlight %}

#### Apache
{% highlight bash %}
RewriteCond %{HTTPS}  off
RewriteCond %{HTTP:X-Forwarded-Proto} !https [NC]
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [L,R=301,NC]
{% endhighlight %}

### How to redirect HTTPS to HTTP

#### NGINX

{% highlight bash %}
server {
  listen 443;
  server_name example.com;
  return 301 http://example.com$request_uri;
}
{% endhighlight %}

#### Apache
{% highlight bash %}
RewriteCond %{HTTPS}  on
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteRule ^(.*)$ http://%{HTTP_HOST}/$1 [L,R=301,NC]
{% endhighlight %}

### Redirect non-www to www

#### NGINX

{% highlight bash %}
server {
  server_name example.com;
  return 301 $scheme://www.example.com$request_uri;
}
{% endhighlight %}

#### Apache
{% highlight bash %}
RewriteEngine on
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteRule ^(.*)$ http://www.example.com/$1 [L,R=301,NC]
{% endhighlight %}

### Redirect www to non-www

#### NGINX

{% highlight bash %}
server {
  server_name www.example.com;
  return 301 $scheme://example.com$request_uri;
}
{% endhighlight %}

#### Apache
{% highlight bash %}
RewriteEngine on
RewriteCond %{HTTP_HOST} ^www.example.com [NC]
RewriteRule ^(.*)$ http://example.com/$1 [L,R=301,NC]
{% endhighlight %}
