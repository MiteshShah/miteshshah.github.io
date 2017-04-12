---
layout: post
title: Setup User Account
author:
modified:
comments: true
categories: devops/lemp
excerpt: "Step By Step Guide to Create/Setup User Accounts"
tags: [SysAdmin, Devops, LEMP, Ubuntu, Debian]
image:
  url:
  alt: Setup User Account
  title: Setup User Account
  feature:
date: 2017-04-12T11:31:48+05:30
---

{% include _toc.html %}

#### Create New User Account

{% highlight bash %}
$ sudo useradd -m -s /bin/bash mitesh
$ sudo chmod 750 /home/mitesh
{% endhighlight %}

#### Grant NGINX Read Permissions

{% highlight bash %}
$ sudo usermod -G mitesh www-data
{% endhighlight %}

#### Setup Custom Bash Prompt PS1

{% highlight bash %}
$ sudo echo 'PS1="\`if [ \$? = 0 ]; then echo \[\e[37m\]^_^[\u@\H:\w]\\$ \[\e[0m\]; else echo \[\e[31m\]O_O[\u@\H:\w]\\$ \[\e[0m\]; fi\`"' >> /home/mitesh/.bashrc
{% endhighlight %}


#### Setup Directory

{% highlight bash %}
$ su - mitesh
$ mkdir -p /home/mitesh/vhosts/{htdocs,ssl,conf,logs}
{% endhighlight %}
