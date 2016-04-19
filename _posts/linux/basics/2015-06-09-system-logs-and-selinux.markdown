---
layout: post
title: "System Logs and SELinux"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about system logs, Network Time Protocol (NTP) and SELinux"
tags: [Linux, Linux Basics, Logs, NTP, SELinux]
image:
  feature:
date: 2015-06-09T10:58:23+05:30
---

{% include _toc.html %}


### System Logs
* Red Hat Enterprise Linux Systems Provides A Central Logging Facility
* That Allows All The Applications To Stores The Messages, Errors & Debugging Information In A Central Manageable Place.

* Logging Daemons: `rsyslog`
* Configuration File:	`/etc/rsyslog.conf`
* Generate Log Messages:	`logger(1)`



#### Monitoring System Logs
* `/var/log/dmesg` This Log file Contains The Kernel Messages That Were Raised During The Boot Process.

* `/var/log/messages` This is the standard system log file,
which contains the messages from the system softwares, non-kernel boot issues & messages that go to the dmesg. Readable By root Only.

* `/var/log/maillog` This Logfile Contains The Mail Systems Messages & Errors. Readable By root Only.

* `/var/log/secure` This Logfile Contains The Security Related Messages & Errors,
Such As Login, Tcp_Wrappers & Xinetd. Readable By root Only.

* `/var/log/audit/audit.log` This Logfile Contains The Audited Messages From The Kernel Including SELinux Related Messages.
Use `ausearch` & `aureport` To View This Log file.

**Examples:**
{% highlight bash %}
[root@Matrix ~]# ausearch -ui 501	[root@Matrix ~]# ausearch -gi 501
[root@Matrix ~]# ausearch -sv yes	[root@Matrix ~]# ausearch -sv no

[root@Matrix ~]# aureport --auth	[root@Matrix ~]# aureport --login

[mitesh@Matrix ~]$ logger This is logger command testing.
[root@Matrix log]# tail -n1 /var/log/messages
Dec 26 16:22:51 Matrix mitesh: This is logger command testing.
{% endhighlight %}


### Network Time Protocol

* Daemons:	`ntpd`
* Services:	`chkconfig ntpd on | off`
  `service ntpd status | start | stop | restart`

**Configuration Files:**

* `/etc/ntp.conf`

**Configuration Tools:**

* CLI:	`ntpq`	`ntpdate`
* GUI:	`system-config-date`
  `System -> Administration -> Date & Time`

  * Can set Date/Time Manually or use NTP
  * Additional NTP Servers can be added
  * Can use Local Time or UTC

**Examples:**
{% highlight bash %}
[root@Matrix ~]# ntpq -c peers
[root@Matrix ~]# ntpdate -u ipaddress-of-ntp-server
[root@Matrix ~]# ntpdate -s ipaddress-of-ntp-server
{% endhighlight %}


### SELinux
* The Traditional Unix Security Model Is A Discretionary Access Control (DAC) System.
* In DAC System Users Are Allowed To Modify Access Control On Files They Own.
* So The NSA Agents Often Uses chmod 777 ~ Easy Way To Share Their Files With Other NSA Agents.
* So By Using DAC, NSA Agents Makes Confidential Information Available For The Rest Of The World.

* The NSA Develop A Mandatory Access Control (MAC) System.
* The NSA Take The Advantage Of The Open Source Linux Kernel & Developed A Set Of Patches To Enable MAC.
* This Patches Are Known As Security Enhanced Linux (SELinux).


**SELinux:**

* Kernel Level Security System
* All The Files & Processes Has A Security Context
* Any Action Not Explicitly Allowed Is Denied By Default

**SELinux Modes:**

* `Enforcing`:	SELinux Security Policy Is Enforced
* `Permissive`:	SELinux Prints Warnings Instead Of Enforcing
* `Disable`:	SELinux Is Disable

**SELinuxTYPE:**

* Targeted	Targeted Processes Are Protected
* MLS	Multi Level Security Protection

**SELinux Configuration:**

* `/etc/selinux/config`
* `/etc/sysconfig/selinux -> ../selinux/config`

**SELinux Logs:**

* `/var/log/audit/audit.log`
* `/var/log/messages`



**SELinux Packages:**

* `yum install policycoreutils-gui`
* `yum install setroubleshoot`

**SELinux Commands:**

* `getenforce`	Display System Current SELinux Mode
* `setenforce`	Toggle Between The Enforcing(1) & Permissive(0) Mode

* `sestatus`	SELinux Status Tool

* `sealert`	CLI & GUI SELinux Troubleshoot Client Tool
* `sealert` -b	Application -> System Tools -> SELinux Troubleshooter

* `system-config-selinux`
  `System -> Administration -> SELinux Management`

**Kernel Arguments**

* `selinux=0 | 1`
* `enforcing=0 | 1`


**Examples:**
{% highlight bash %}
[root@Matrix ~]# getenforce 		[root@Matrix ~]# sestatus
Enforcing				SELinux status:			enabled
SELinuxfs mount:		/selinux
[root@Matrix ~]# setenforce 0		Current mode:			enforcing
[root@Matrix ~]# getenforce 		Mode from config file:		enforcing
Permissive				Policy version:			24
Policy from config file:	targeted
[root@Matrix ~]# setenforce 1
[root@Matrix ~]# getenforce
Enforcing
{% endhighlight %}
