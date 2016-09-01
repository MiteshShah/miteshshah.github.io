---
layout: post
title: How to Configure Nagios Monitoring System
author:
modified:
comments: true
categories: devops/nagios
excerpt: "Step by step guide to Configure Nagios"
tags: [Devops, Nagios, NRPE, Ubuntu]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/18166139/0917267c-7067-11e6-8efb-58950b52fab4.jpg
  alt: How to Configure Nagios Monitoring System
  title: How to Configure Nagios Monitoring System
  feature:
date: 2016-09-01T23:51:10+05:30
---


{% include _toc.html %}

### Configure Nagios

{% highlight bash %}
$ vim /usr/local/nagios/etc/nagios.cfg
# Add following lines on cfg_dir block
cfg_dir=/usr/local/nagios/etc/contacts
cfg_dir=/usr/local/nagios/etc/contactgroups
cfg_dir=/usr/local/nagios/etc/services
cfg_dir=/usr/local/nagios/etc/commands
cfg_dir=/usr/local/nagios/etc/hosts
cfg_dir=/usr/local/nagios/etc/hostgroups
{% endhighlight %}

* Create Nagios Directory

{% highlight bash %}
$ mkdir /usr/local/nagios/etc/{contacts,contactgroups,services,commands,hosts,hostgroups}
{% endhighlight %}


#### Nagios Contacts

{% highlight bash %}
$ vim /usr/local/nagios/etc/contacts/alerts.cfg
define contact{
        contact_name                    ops
        alias                           Ops
        service_notification_period     24x7
        host_notification_period        24x7
        service_notification_options    w,u,c,r
        host_notification_options       d,u,r
        service_notification_commands   notify-service-by-email
        host_notification_commands      notify-host-by-email
        email                           alerts@example.com
}
{% endhighlight %}

#### Nagios Contact Groups
{% highlight bash %}
$ vim /usr/local/nagios/etc/contactgroups/oncall-admin.cfg
define contactgroup{
        contactgroup_name      oncall-admins
        alias                  On-call Admins
        members                ops
 }
{% endhighlight %}


#### Nagios Hosts
{% highlight bash %}
$ vim /usr/local/nagios/etc/hosts/example.com
define host {
        use                             linux-server
        host_name                       example.com
        alias                           example.com
        address                         example.com
        max_check_attempts              5
        check_period                    24x7
        notification_interval           30
        notification_period             24x7
	notification_options    	d,u,r
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/hosts/test.com
define host {
        use                             linux-server
        host_name                       test.com
        alias                           test.com
        address                         test.com
        max_check_attempts              5
        check_period                    24x7
        notification_interval           30
        notification_period             24x7
	notification_options    	d,u,r
}
{% endhighlight %}

#### Nagios Host Groups

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/http.cfg
define hostgroup{
        hostgroup_name  HTTP
        alias           HTTP
        members         example.com,test.com
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/https.cfg
define hostgroup{
        hostgroup_name  HTTPS
        alias           HTTPS
        members         example.com
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/remote_mysql.cfg
define hostgroup{
        hostgroup_name  REMOTE_MYSQL
        alias           REMOTE MYSQL
        members         example.com,test.com
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/hostgroups/remote_nginx.cfg
define hostgroup{
        hostgroup_name  REMOTE_NGINX
        alias           REMOTE NGINX
        members         example.com,test.com
}
{% endhighlight %}

{% highlight bash %}
$ define hostgroup{
        hostgroup_name  REMOTE
        alias           Remote Linux Servers
        members         example.com,test,com
}
{% endhighlight %}


#### Nagios Services

{% highlight bash %}
$ vim /usr/local/nagios/etc/services/http.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             HTTP
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  HTTP
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_http
}
{% endhighlight %}

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
$ vim /usr/local/nagios/etc/services/remote_mysql.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             Remote MYSQL
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  REMOTE_MYSQL
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_nrpe!check_mysql
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/services/remote_nginx.cfg
define service{
        use                             local-service         ; Name of service template to use
        service_description             Remote NGINX
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  REMOTE_NGINX
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_nrpe!check_nginx
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/services/remote.cfg
define service{
        use                             generic-service         ; Name of service template to use
        service_description             Root Partition
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  REMOTE
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_nrpe!check_hda1
}
define service{
        use                             generic-service         ; Name of service template to use
        service_description             Current Load
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  REMOTE
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_nrpe!check_load
}
define service{
        use                             generic-service         ; Name of service template to use
        service_description             Memory Swap
        is_volatile                     0
        check_period                    24x7
        max_check_attempts              3
        normal_check_interval           3
        retry_check_interval            1
        contact_groups                  oncall-admins
        hostgroup_name                  REMOTE
        notification_interval           30
        notification_period             24x7
        notification_options            c,r
        check_command                   check_nrpe!check_memory
}
#define service{
#        use                             generic-service         ; Name of service template to use
#        service_description             Total Process
#        is_volatile                     0
#        check_period                    24x7
#        max_check_attempts              3
#        normal_check_interval           3
#        retry_check_interval            1
#        contact_groups                  oncall-admins
#        hostgroup_name                  REMOTE
#        notification_interval           30
#        notification_period             24x7
#        notification_options            c,r
#        check_command                   check_nrpe!check_total_procs
#}
{% endhighlight %}


#### Nagios Commands
{% highlight bash %}
$ cp -av /usr/local/nagios/etc/objects/commands.cfg /usr/local/nagios/etc/commands/
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/commands/https.cfg
define command{
        command_name    check_https
        command_line    $USER1$/check_http -H $HOSTADDRESS$ -C 15
}
{% endhighlight %}

{% highlight bash %}
$ vim /usr/local/nagios/etc/commands/nrpe.cfg
define command{
        command_name check_nrpe
        command_line $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}
{% endhighlight %}
