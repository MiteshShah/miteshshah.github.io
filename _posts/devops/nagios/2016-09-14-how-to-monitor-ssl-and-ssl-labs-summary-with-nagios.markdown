---
layout: post
title: How to Monitor SSL & SSL Labs Summary With Nagios
author:
modified:
comments: true
categories: devops/nagios
excerpt: "Monitor SSL Labs Results with Nagios Monitoring System"
tags: [Devops, Nagios, Ubuntu, HTTPS, SSL, SSLLABS]
image:
  url:
  alt: How to Monitor SSL & SSL Labs Summary With Nagios
  title: How to Monitor SSL & SSL Labs Summary With Nagios
  feature:
date: 2016-09-14T23:37:59+05:30
---


{% include _toc.html %}

### Prerequisites
{% highlight bash %}
$ apt-get -y install libwww-perl libjson-perl

$ wget -O /usr/local/nagios/libexec/check_sslscan https://www.unixadm.org/software/nagios-stuff/checks/check_sslscan
$ chmod  a+x /usr/local/nagios/libexec/check_sslscan
{% endhighlight %}

### Nagios Host Groups

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/https.cfg
define hostgroup{
        hostgroup_name  HTTPS
        alias           HTTPS
        members         example.com
}
{% endhighlight %}

### Nagios Services

{% highlight bash %}
$ vim /usr/local/nagios/etc/services/https.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             HTTPS
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  HTTPS
        notification_interval           30
        notification_period             24x7
        notification_options            w,c,r
        check_command                   check_https
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/services/ssllabs.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             SSL Labs Summary
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  HTTPS
        notification_interval           30
        notification_period             24x7
        notification_options            w,c,r
        check_command                   check_sslscan
}
{% endhighlight %}

### Nagios Commands

{% highlight bash %}
$ vim /usr/local/nagios/etc/commands/https.cfg
define command{
        command_name    check_https
        command_line    $USER1$/check_http -H $HOSTADDRESS$ -C 15 --sni
}

{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/commands/sslscan.cfg
define command{
        command_name    check_sslscan
        command_line    $USER1$/check_sslscan -H $HOSTADDRESS$ -a 168 #-x
}
{% endhighlight %}
