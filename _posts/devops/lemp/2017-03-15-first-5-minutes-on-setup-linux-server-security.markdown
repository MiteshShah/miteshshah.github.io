---
layout: post
title: First 5 Minutes on Setup Linux Server Security
author:
modified:
comments: true
categories: devops/lemp
excerpt: "My security philosophy is simple: adopt principles that will protect you from the most frequent attack vectors. If you use your first 5 minutes on a server wisely, I believe you secure your server in much better way. "
tags: [SysAdmin, DevOps, LEMP, Ubuntu, Debian,]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/23937334/1300d066-097d-11e7-9a74-df7a6eec2277.jpg
  alt: First 5 Minutes on Setup Linux Server Security
  title: First 5 Minutes on Setup Linux Server Security
  feature:
date: 2017-03-15T12:26:06+05:30
---


{% include _toc.html %}

### Setup FQDN Hostname

{% highlight bash %}
$ sudo vim /etc/hostname
srv1.example.com
{% endhighlight %}

### Setup Timezone

* To know more why you need to setup timezone <a href="http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html"> Click Here </a>

{% highlight bash %}
$ sudo timedatectl set-timezone Etc/UTC
{% endhighlight %}


### Setup UMASK

* To know more about UMASK <a href="/linux/basics/advanced-topics-in-users-groups-and-permissions/#default-permissions"> Click Here </a>
* By default - Ubuntu will allow to go inside another user home directory and read the data.
* By changing UMASK any new user account home directory permission set from 755 to 750
* More information - <a href="https://wiki.ubuntu.com/SecurityTeam/Policies#Permissive_Home_Directory_Access"> Permissive Home Directory Access
 </a>

{% highlight bash %}
$ sudo echo umask 0027 >> /etc/profile
{% endhighlight %}

### Update System Packages

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get dist-upgrade
{% endhighlight %}

### Setup NTP

{% highlight bash %}
$ sudo apt-get install ntp
{% endhighlight %}

### Setup User & PS1

{% highlight bash %}
$ sudo useradd user -ms /bin/bash
$ sudo passwd user
$ sudo chmod 0750 /home/user
$ sudo echo 'PS1="\`if [ \$? = 0 ]; then echo \[\e[37m\]^_^[\u@\H:\w]\\$ \[\e[0m\]; else echo \[\e[31m\]O_O[\u@\H:\w]\\$ \[\e[0m\]; fi\`"' >> /home/user/.bashrc
{% endhighlight %}

### Lockdown SSH

* Configure ssh to prevent password & root logins

{% highlight bash %}
# Change Following Values
$ sudo vim /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no

# Restart SSH
$ sudo service ssh restart
{% endhighlight %}

### Enable Automatic Security Updates

* Update the file to look like below.
* You should probably keep updates disabled and stick with security updates only.
{% highlight bash %}
$ sudo apt-get install unattended-upgrades
$ sudo vim /etc/apt/apt.conf.d/10periodic
// Automatically upgrade packages from these (origin:archive) pairs
Unattended-Upgrade::Allowed-Origins {
//	"${distro_id}:${distro_codename}";
  	"${distro_id}:${distro_codename}-security";
//	"${distro_id}:${distro_codename}-updates";
//	"${distro_id}:${distro_codename}-proposed";
//	"${distro_id}:${distro_codename}-backports";
};
{% endhighlight %}

### Install Logwatch To Keep An Eye On Things

{% highlight bash %}
$ sudo apt-get install logwatch
$ sudo vim /etc/cron.daily/00logwatch
#!/bin/bash

#Check if removed-but-not-purged
test -x /usr/share/logwatch/scripts/logwatch.pl || exit 0

#execute
/usr/sbin/logwatch --output mail --mailto Mr.Miteshah@gmail.com --detail high

#Note: It's possible to force the recipient in above command
#Just pass --mailto address@a.com instead of --output mail
{% endhighlight %}
