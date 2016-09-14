---
layout: post
title: How to Monitor Domain Expire Date With Nagios
author:
modified:
comments: true
categories: devops/nagios
excerpt: "Check Domain Expire Date With Nagios Monitoring System"
tags: [Devops, Nagios, Ubuntu, Domain]
image:
  url:
  alt: How to Monitor Domain Expire Date With Nagios
  title: How to Monitor Domain Expire Date With Nagios
  feature:
date: 2016-09-14T23:36:22+05:30
---


{% include _toc.html %}

### Prerequisites
{% highlight bash %}
$ wget -O /usr/local/nagios/libexec/check_domain.sh https://raw.githubusercontent.com/glensc/monitoring-plugin-check_domain/master/check_domain.sh
$ chmod  a+x /usr/local/nagios/libexec/check_domain.sh
{% endhighlight %}

### Nagios Host Groups

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/check_domain.cfg
define hostgroup{
        hostgroup_name  CHECK_DOMAIN
        alias           Domain Expiry Check
        members         example.com,test.com
}
{% endhighlight %}

### Nagios Services
{% highlight bash %}
$ vim /usr/local/nagios/etc/services/check_domain.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             Domain Expiry
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  CHECK_DOMAIN
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_domain
}
{% endhighlight %}

### Nagios Commands

{% highlight bash %}
$ vim /usr/local/nagios/etc/commands/domain.cfg
define command{
        command_name    check_domain
        command_line    $USER1$/check_domain.sh -d $HOSTADDRESS$ -w30 -c 15
#-a 1 -C /usr/local/nagios/cache -w 30 -c 15
}
{% endhighlight %}

<img alt="Domain Expiry" src="https://cloud.githubusercontent.com/assets/1223371/18525079/b0238dfe-7ad8-11e6-939c-8579fe71826c.png">
