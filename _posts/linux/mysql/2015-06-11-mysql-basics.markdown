---
layout: post
title: "MySQL Basics"
author:
modified:
comments: true
categories: linux/mysql
excerpt: "Newbie Guide - All about MySQL Database Server"
tags: [Linux, MySQL]
image:
  feature:
date: 2015-06-11T11:47:28+05:30
---

{% include _toc.html %}

### Install MySQL Server

### Debian/Ubuntu
{% highlight bash %}
[mitesh@Matrix ~]$ sudo apt-get update && sudo apt-get install -y mysql-server
{% endhighlight %}

### Redhat/CentOS
{% highlight bash %}
[mitesh@Matrix ~]$ sudo yum install -y mysql-server
{% endhighlight %}

### Important Files

#### Configuration File

* `/etc/mysql/my.cnf`		The Main MySQL Configuration File

#### Log Files

* `/var/log/mysql/`			Default Log Directory For MySQL
* `/etc/logrotate.d/mysql-server`	Log Rotation Policy For MySQL Logs


### MySQL Tricks

#### Reset MySQL Password
{% highlight bash %}
[mitesh@Matrix ~]$ sudo mysqladmin -u root password NEWPASSWORD
{% endhighlight %}

**NOTE!**: Add `skip-grant-tables` in `/etc/mysql/my.cnf` under the `[mysqld]` section and restart mysql server.
{: .notice}

#### Create MySQL Database
{% highlight bash %}
[mitesh@Matrix ~]$ mysql -u USERNAME -pPASSWORD -e 'create database DB_NAME'
{% endhighlight %}

#### Remove MySQL Database
{% highlight bash %}
[mitesh@Matrix ~]$ mysql -u USERNAME -pPASSWORD -e 'drop database DB_NAME'
{% endhighlight %}

#### Create MySQL USER
{% highlight bash %}
[mitesh@Matrix ~]$ create user 'USERNAME'@'localhost' identified by 'PASSWORD';
{% endhighlight %}

#### Grant Privileges
{% highlight bash %}
[mitesh@Matrix ~]$ grant all privileges on `DB_NAME`.* to 'USERNAME'@'localhost';
{% endhighlight %}

#### Grant File Privileges
{% highlight bash %}
[mitesh@Matrix ~]$ grant file on . to 'USERNAME'@'localhost';
[mitesh@Matrix ~]$ grant file on *.* to 'USERNAME'@'localhost';
{% endhighlight %}

#### Update User Passwords
{% highlight bash %}
[mitesh@Matrix ~]$ UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
[mitesh@Matrix ~]$ set password for 'USERNAME'@'localhost' = password('PASSWORD');
{% endhighlight %}

#### Backup MySQL Database
{% highlight bash %}
[mitesh@Matrix ~]$ mysqldump -u USERNAME -h HOSTNAME -pPASSWORD $DB_NAME
{% endhighlight %}

#### Backup All MySQL Database
{% highlight bash %}
[mitesh@Matrix ~]$ mysqldump -u USERNAME -h HOSTNAME -pPASSWORD --all-databases
{% endhighlight %}

#### Restore MySQL Database
{% highlight bash %}
[mitesh@Matrix ~]$ mysql -u USERNAME -pPASSWORD DB_NAME < Database.sql
{% endhighlight %}

#### Export Specific Tables Only
{% highlight bash %}
[mitesh@Matrix ~]$ mysql DB_NAME -u USERNAME -p -e 'show tables like "wp_5%"' | grep -v Tables_in | xargs mysqldump DB_NAME -u root -p > SQLFILE.sql
{% endhighlight %}
