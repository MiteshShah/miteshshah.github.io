---
layout: post
title: LEMP Stack - Conventions & File System Layout
author:
modified:
comments: true
categories: devops/lemp
excerpt: "A place for everything and everything in its place!"
tags: [SysAdmin, Devops, LEMP, Ubuntu, Debian]
image:
  url:
  alt: LEMP Stack - Conventions & File System Layout
  title: LEMP Stack - Conventions & File System Layout
  feature:
date: 2017-03-15T15:59:34+05:30
---


{% include _toc.html %}

### File System Layout

#### NGINX

* `/etc/nginx` - Main NGINX Configuration Directory
* `/etc/nginx/nginx.conf` - Main NGINX Configuration File
* `/etc/nginx/sites-available/` - NGINX Configuration For Websites
* `/etc/nginx/sites-enabled/` - Symbolic Link for Active Websites
* `/etc/logrotate.d/nginx` - NGINX Log Rotation Configuration File
* `/var/log/nginx/` - NGINX Log Directory

#### PHP7

* `/etc/php/7.1/` - Main PHP7 Configuration Directory
* `/etc/php/7.1/fpm/php.ini` - Main PHP7 Configuration File
* `/etc/php/7.1/fpm/php-fpm.conf` - PHP7 FPM Related Settings for WWW Pool
* `/etc/php/7.1/fpm/pool.d/www.conf` - PHP7 WWW Pool Settings
* `/etc/php/7.1/fpm/pool.d/debug.conf` - PHP7 XDEBUG Pool Settings
* `/etc/php/7.1/fpm/pool.d/user.conf` - PHP7 Specific User Pool Settings
* `/var/log/php/` - PHP7 Log Directory

#### MySQL

* `/etc/mysql/` - Main MySQL Configuration Directory
* `/etc/mysql/my.cnf` - MySQL Configuration File
* `/root/.my.cnf` - MySQL root Username/Password File
* `/var/log/syslog` - MySQL Logs File

#### Website

* `/home/user/vhosts/` - Main WebRoot For user Account
* `/home/user/vhosts/example.com/htdocs/` - Webroot for example.com website
* `/home/user/vhosts/example.com/conf/` - Any `*.conf` file inside this will be added on NGINX Rules
* `/home/user/vhosts/example.com/ssl/` - SSL Certificate For example.com
* `/home/user/vhosts/example.com/logs/` - NGINX Logs for example.com
