---
layout: post
title: How to Install WordPress on CentOS7
author:
modified:
comments: true
categories: linux/centos
excerpt: "In this guide, we will demonstrate how to get a WordPress instance set up with an Apache web server on CentOS 7."
tags: [Linux, CentOS, WordPress]
image:
  url:
  alt: How to Install WordPress on CentOS7
  title: How to Install WordPress on CentOS7
  feature:
date: 2016-03-15T15:19:08+05:30
---


{% include _toc.html %}

### Prerequisites

* <a href="/linux/centos/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos7/"> Setup LAMP Stack </a>

### Setup Database & DB User

{% highlight bash %}
$ mysql -u root -p
MariaDB [(none)]> CREATE DATABASE example_com;
MariaDB [(none)]> CREATE USER example_user@localhost IDENTIFIED BY 'E1hciAx';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON example_com.* TO example_user@localhost IDENTIFIED BY 'E1hciAx';
MariaDB [(none)]> FLUSH PRIVILEGES;
{% endhighlight %}

### Setup Virtual Host

{% highlight bash %}
$ vim /etc/httpd/conf/httpd.conf +$
<VirtualHost 1.1.1.1:80>
  ServerName example.com
  SetEnv realm prod
  DocumentRoot /var/www/vhosts/example.com
  ErrorLog logs/example.com-error_log
  CustomLog logs/example.com-access_log enhanced
  <Directory /var/www/vhosts/example.com>
    Options Indexes FollowSymLinks
    Require all granted
    AllowOverride All
  </Directory>
</VirtualHost>
{% endhighlight %}

### Download WordPress

{% highlight bash %}
$ wget http://wordpress.org/latest.tar.gz
$ tar -zxvf latest.tar.gz -C /var/www/vhosts/example.com/ --strip-components=1
{% endhighlight %}

### Fix Permission

{% highlight bash %}
$ sudo chown -R apache:apache /var/www/vhosts/example.com/
{% endhighlight %}
