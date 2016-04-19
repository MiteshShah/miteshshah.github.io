---
layout: post
title: "MyCLI - MySQL MariaDB and Percona Auto-completion and Syntax Highlighting Tool"
author:
modified:
comments: true
categories: linux/mysql
excerpt: "Mycli is a command line interface for MySQL, MariaDB, and Percona with auto-completion and syntax highlighting."
tags: [Linux, Database, MySQL, Mariadb, Percona, MyCLI]
image:
  url: http://mycli.net/images/main.gif
  alt: MyCLI - MySQL MariaDB and Percona Auto-completion and Syntax Highlighting Tool
  title:  MyCLI - MySQL MariaDB and Percona Auto-completion and Syntax Highlighting Tool
  feature:
date: 2015-07-30T23:26:32+05:30
---

{% include _toc.html %}

### Install MyCLI

#### Linux Based OS
{% highlight bash %}
$ pip install mycli
{% endhighlight %}

#### Mac OS
{% highlight bash %}
$ brew install mycli
{% endhighlight %}

**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}

#### MyCLI Examples
{% highlight bash %}
$  mycli DB_NAME
$ mycli -h localhost -u root DB_NAME
$ mycli mysql://mitesh@localhost:3306/DB_NAME
{% endhighlight %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

#### More Information about MyCLI

1. <a href="http://mycli.net/"> MyCLI Website </a>
1. <a href="https://github.com/dbcli/mycli"> MyCLI Github Repository </a>
