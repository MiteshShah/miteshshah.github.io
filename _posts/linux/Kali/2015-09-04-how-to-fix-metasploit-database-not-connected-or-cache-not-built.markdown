---
layout: post
title: How to Fix Metasploit Database Not Connected or Cache Not Built
author:
modified:
comments: true
categories: linux/Kali
excerpt: "This is a short post explaining how to deal with Metasploit instance not connected to its Database"
tags: [Kali, Linux, Metasploit, PostgreSQL]
image:
  url:
  alt:
  title:
  feature:
date: 2015-09-04T14:17:56+05:30
---

{% include _toc.html %}

### Start Required Services

* Metasploit uses PostgreSQL as its database so it needs to be launched first.

{% highlight bash %}
$ sudo service postgresql start
{% endhighlight %}

### Initialise the Metasploit PostgreSQL Database

* With PostgreSQL up and running, we next need to create and initialize the msf database.
{% highlight bash %}
$ sudo msfdb init
{% endhighlight %}

### Launch msfconsole in Kali

{% highlight bash %}
$ sudo msfconsole
msf > db_status
[*] postgresql connected to msf3
{% endhighlight %}

### Fix Metasploit Cache Issue
{% highlight bash %}
msf > search wordpress
[!] Database not connected or cache not built, using slow search

# Rebuid Cache
# It takes some time for the cache to be rebuild
msf> db_rebuild_cache
{% endhighlight %}
