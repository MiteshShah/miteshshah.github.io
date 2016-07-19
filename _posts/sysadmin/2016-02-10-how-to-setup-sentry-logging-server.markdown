---
layout: post
title: How to Setup Sentry Logging Server
author:
modified:
comments: true
categories: sysadmin
excerpt: "Sentry provides real-time crash reporting for your web apps, mobile apps, and games."
tags: [SysAdmin]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/12943884/27d77312-d00a-11e5-9cbc-0a8f0fe4317d.gif
  alt: How to Setup Sentry Logging Server
  title: How to Setup Sentry Logging Server
  feature:
date: 2016-02-10T15:19:43+05:30
---


{% include _toc.html %}
<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Setup Repository

{% highlight bash %}
# Redis Repository
$ sudo add-apt-repository ppa:chris-lea/redis-server

# Postgresql Repository
$ sudo echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" >  /etc/apt/sources.list.d/pgdg.list
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
{% endhighlight %}

### Update Packages

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get dist-upgrade
{% endhighlight %}

### Install Packages

{% highlight bash %}
$ sudo apt-get install python-setuptools python-pip python-dev libxslt1-dev libxml2-dev libz-dev libffi-dev libssl-dev libpq-dev libyaml-dev redis-server postgresql-9.4 nginx-full libjpeg-dev
{% endhighlight %}

### Create Sentry User

{% highlight bash %}
# Create sentry user and add to the sudo group.
$ sudo adduser sentry
$ sudo adduser sentry sudo
{% endhighlight %}

### Setting up an Environment

{% highlight bash %}
$ sudo pip install -U virtualenv
{% endhighlight %}

* Once that’s done, choose a location for the environment, and create it with the virtualenv command. For our guide, we’re going to choose `/home/sentry`

{% highlight bash %}
$ su - sentry
$ virtualenv /home/sentry/
$ source /home/sentry/bin/activate
(sentry) $ echo "source /home/sentry/bin/activate" >> ~/.bashrc
{% endhighlight %}

### Install Sentry

{% highlight bash %}
(sentry) $ pip install -U sentry
{% endhighlight %}

### Database Setup

{% highlight bash %}
(sentry) $ sudo su - postgres
$ createdb sentry_db
$ createuser sentry_user --pwprompt
$ psql -d template1 -U postgres
psql (9.4.5)
Type "help" for help.

template1=# GRANT ALL PRIVILEGES ON DATABASE sentry_db to sentry_user;
GRANT
template1=# \q
postgres@sent01:~$ exit
{% endhighlight %}

### Initializing the Configuration

* Now you’ll need to create the default configuration.
* To do this, you’ll use the `init` command You can specify an alternative configuration path as the argument to init, otherwise it will use the default of `~/.sentry`.

{% highlight bash %}
(sentry) $ sentry init
{% endhighlight %}

* The configuration for the server is based on `sentry.conf.server`, which contains a basic Django project configuration, as well as the default Sentry configuration values.

{% highlight bash %}
(sentry) $ vim ~/.sentry/sentry.conf.py
DATABASES = {
    'default': {
        'ENGINE': 'sentry.db.postgres',
        'NAME': 'sentry_db',
        'USER': 'sentry_user',
        'PASSWORD': '******',
        'HOST': 'localhost',
        'PORT': '',
    }
}
{% endhighlight %}


### Running Migrations

* Sentry provides an easy way to run migrations on the database on version upgrades.
* Before running it for the first time you’ll need to make sure you’ve created the database.



* Once done, you can create the initial schema using the `upgrade` command

{% highlight bash %}
(sentry) $ SENTRY_CONF=~/.sentry sentry upgrade
{% endhighlight %}

* Next up you’ll need to create the first user, which will act as a superuser

{% highlight bash %}
(sentry) $ SENTRY_CONF=~/.sentry sentry createuser
{% endhighlight %}

* All schema changes and database upgrades are handled via the upgrade command, and this is the first thing you’ll want to run when upgrading to future versions of Sentry.

### Starting the Web Service

* Sentry provides a built-in webserver (powered by uWSGI) to get you off the ground quickly, also you can setup Sentry as WSGI application, in that case skip to section Running Sentry as WSGI application.

#### Start Builtin Webserver

{% highlight bash %}
(sentry) $ SENTRY_CONF=~/.sentry sentry start &
{% endhighlight %}

#### Start Workers

* A large amount of Sentry’s work is managed via background workers.
* These need run in addition to the web service workers

{% highlight bash %}
(sentry) $ SENTRY_CONF=~/.sentry sentry celery worker &
{% endhighlight %}

* See <a href="https://docs.getsentry.com/on-premise/server/queue/">Asynchronous Workers</a> for more details on configuring workers.

#### Start Cron Process

* Sentry also needs a cron process which is called `celery beat`

{% highlight bash %}
(sentry) $ SENTRY_CONF=~/.sentry sentry celery beat &
{% endhighlight %}

* You should now be able to test the web service by visiting <a href="http://localhost:9000">http://localhost:9000</a>.

### Setup a Reverse Proxy

* By default, Sentry runs on port 9000. Even if you change this, under normal conditions you won’t be able to bind to port 80.
* To get around this (and to avoid running Sentry as a privileged user, which you shouldn’t), we recommend you setup a simple web proxy.

* You’ll use the builtin HttpProxyModule within Nginx to handle proxying:
{% highlight bash %}
$ sudo vim /etc/nginx/sites-available/sentry
server {
	listen 80 default_server;
	server_name sentry.example.com;

	location / {
		proxy_pass         http://localhost:9000;
		proxy_redirect     off;

		proxy_set_header   Host              $host;
		proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
		proxy_set_header   X-Forwarded-Proto $scheme;
	}
}
{% endhighlight %}

#### Restart NGINX

{% highlight bash %}
(sentry) $ sudo ln -s /etc/nginx/sites-available/sentry /etc/nginx/sites-enabled/
(sentry) $ sudo service nginx restart
{% endhighlight %}

### More Information

To know more Information refer <a href="https://docs.getsentry.com/on-premise/server/installation/">official documents</a>.
