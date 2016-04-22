---
layout: post
title: How to Install Linux Apache MySQL PHP (LAMP) Stack on CentOS7
author:
modified:
comments: true
categories: linux/centos
excerpt: "A LAMP (Linux, Apache, MySQL, PHP) stack is a common web stack used for hosting web content. This guide shows you how to install a LAMP stack on a CentOS 7 server."
tags: [Linux, CentOS, Apache, Httpd, MySQL, Mariadb, PHP]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/13773008/2300f0f2-eabd-11e5-8593-9679e124524a.png
  alt: How to Install Linux Apache MySQL PHP (LAMP) Stack on CentOS7
  title: How to Install Linux Apache MySQL PHP (LAMP) Stack on CentOS7
  feature:
date: 2016-03-15T14:46:28+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### System Update

{% highlight bash %}
# Update system packages
$ sudo yum update
{% endhighlight %}

### Setup/Install Repository

{% highlight bash %}
# If you want to use MySQL over MariaDB
# $ sudo yum install http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

# PHP5.6 Repository
$  sudo yum install https://rhel7.iuscommunity.org/ius-release.rpm
{% endhighlight %}

### Install Apache

{% highlight bash %}
# Install Apache
$ sudo yum install httpd

# Start the Apache/Httpd service
$ sudo systemctl start httpd.service

# Enable Apache/Httpd service at boot time
$ sudo systemctl enable httpd.service
{% endhighlight %}

### Install MySQL/MariaDB

{% highlight bash %}
# Install MariaDB
$ sudo yum install mariadb-server

# Start the MariaDB Database
$ sudo systemctl start mariadb.service

# Enable MariaDB service at boot time
$ sudo systemctl enable mariadb.service

# Secure MariaDB
$ mysql_secure_installation
{% endhighlight %}

### Install PHP5.6

{% highlight bash %}
# Install PHP5.6
$ sudo yum install php56u php56u-fpm php56u-mysql php56u-gd php56u-common php56u-xmlrpc php56u-pear php56u-xml

# Restart Apache/Httpd service
$ sudo systemctl restart httpd.service
{% endhighlight %}
