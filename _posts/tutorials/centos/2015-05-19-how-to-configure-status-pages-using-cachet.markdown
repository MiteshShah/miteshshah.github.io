---
layout: post
title: "How to Configure Status Pages Using Cachet"
modified:
categories: tutorials/centos
excerpt: "Beautiful & simple service statuses.
An open source status page system, for everyone."
tags: [Status Page, Cachet, CentOS]
image:
  feature: cachethq.jpg
  credit: cachethq.io
  creditlink: https://cachethq.io/img/main-interface.jpg
date: 2015-05-19T16:19:07+05:30
---



### Requirements

* CentOS 7
* Knowledge of command line

### Automate Setup Cachet Process

{% highlight bash %}
[mitesh@status.example.com ~]$ wget -c https://goo.gl/1IjimY -O status-page.sh
[mitesh@status.example.com ~]$ sudo bash status-page.sh http://status.example.com
{% endhighlight %}

The installation may take a little time, so grab a cup of coffee <i class="fa fa-coffee"></i> and relax.


### Manual process

#### Install avaibale updates
{% highlight bash %}
[mitesh@status.example.com ~]$ yum -y update
{% endhighlight %}

#### Install required repository
{% highlight bash %}
[mitesh@status.example.com ~]$  yum -y install epel-release http://rpms.famillecollet.com/enterprise/remi-release-7.rpm http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
{% endhighlight %}

#### Install required packages
{% highlight bash %}
[mitesh@status.example.com ~]$ yum --enablerepo=remi,remi-php56 install nginx php-fpm php-common php-mcrypt php-mbstring php-apcu php-xml php-pdo php-intl php-mysql php-cli openssl mysql-community-server vim curl git
{% endhighlight %}

#### Setup MySQL
{% highlight bash %}
# Chnage MySQL root password
[mitesh@status.example.com ~]$ sudo mysqladmin -u root password ROOT_PASSWORD
# Store MySQL root password
[mitesh@status.example.com ~]$ echo -e "[client]\nuser=root\npassword=ROOT_PASSWORD" > ~/.my.cnf
# Create cachet database
[mitesh@status.example.com ~]$ mysql -e "create database cachet"
# Create cachet user
[mitesh@status.example.com ~]$ mysql -e "create user 'cachet'@'localhost' identified by 'CACHET_USER_PASSWORD'"
[mitesh@status.example.com ~]$ mysql -e "grant all privileges on cachet.* to 'cachet'@'localhost'"
[mitesh@status.example.com ~]$ mysql -e "flush privileges"
{% endhighlight %}

#### Install composer
{% highlight bash %}
[mitesh@status.example.com ~]$ curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer
{% endhighlight %}

#### Clone cachet repository
{% highlight bash %}
[mitesh@status.example.com ~]$ cd /var/www
[mitesh@status.example.com /var/www]$ git clone https://github.com/cachethq/Cachet.git
[mitesh@status.example.com /var/www/Cachet]$ cd Cachet && cp -v .env.example .env
{% endhighlight %}

* Open `.env` file and update it as shown below
{% highlight bash %}
[mitesh@status.example.com /var/www/Cachet]$ cat .env
APP_ENV=local
APP_DEBUG=true
APP_URL=http://status.example.com
APP_KEY=UyIXoUsiAzvWrnfZwp1DAC50SQHuH5b2

DB_DRIVER=mysql
DB_HOST=127.0.0.1
DB_DATABASE=cachet
DB_USERNAME=cachet
DB_PASSWORD=CACHET_USER_PASSWORD

CACHE_DRIVER=apc
SESSION_DRIVER=apc
QUEUE_DRIVER=database

MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
{% endhighlight %}

#### Setup permission
{% highlight bash %}
# php-fpm running as apache user
[mitesh@status.example.com /var/www/Cachet]$ chown -R apache:apache /var/www/Cachet/
{% endhighlight %}

#### Setup composer
{% highlight bash %}
[mitesh@status.example.com /var/www/Cachet]$ composer install --no-dev -o
{% endhighlight %}

#### Database migration
{% highlight bash %}
[mitesh@status.example.com /var/www/Cachet]$ php artisan migrate
{% endhighlight %}

#### Setup NGINX configuration
{% highlight bash %}
[mitesh@status.example.com /var/www/Cachet]$ cat /etc/nginx/conf.d/cachet.conf
server {
    listen 80;
    server_name status.example.com;

    root /var/www/Cachet/public;
    index index.php;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ \.php$ {
                include fastcgi_params;
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_index index.php;
                fastcgi_keep_conn on;
    }
}
{% endhighlight %}

#### Restart NGINX and PHP-FPM services
{% highlight bash %}
[mitesh@status.example.com ~]$ systemctl start nginx && systemctl start php-fpm
{% endhighlight %}


#### Configure Cachet

* Once installation done open <a href="http://status.example.com"> http://status.example.com </a>
* Now setup CachetHQ as shown in the images below.
![step1](https://cloud.githubusercontent.com/assets/1223371/7701697/5b2060de-fe47-11e4-8ce8-2978430206b2.png)
![step2](https://cloud.githubusercontent.com/assets/1223371/7701698/5b220f60-fe47-11e4-9c9e-d871fda3c506.png)
![step3](https://cloud.githubusercontent.com/assets/1223371/7701699/5b47a54a-fe47-11e4-8b88-dbe110ffa164.png)
