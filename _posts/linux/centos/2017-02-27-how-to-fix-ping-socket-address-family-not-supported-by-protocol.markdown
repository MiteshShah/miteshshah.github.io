---
layout: post
title: How to Fix Ping Socket Address Family Not Supported by Protocol
author:
modified:
comments: true
categories: linux/centos
excerpt: "Recently CentOS7 update fix the iputils package ping command, The ping command no longer works with IPv4 address"
tags: [Linux, CentOS, Ping]
image:
  url:
  alt: How to Fix Ping Socket Address Family Not Supported by Protocol
  title: How to Fix Ping Socket Address Family Not Supported by Protocol
  feature:
date: 2017-02-27T11:09:17+05:30
---


{% include _toc.html %}

* Recently I'd updated CentOS 7 box
* This will updated `iputils` package from `20121221-7.el7` to `20160308-8.el7`
* After the update the `ping` command is no longer to ping any IPv4 address :(

{% highlight bash %}
$ ping google.com
ping: socket: Address family not supported by protocol

$ ping 127.0.0.1
ping: socket: Address family not supported by protocol

$ ping localhost
ping: socket: Address family not supported by protocol

$ ping google.com
ping: socket: Address family not supported by protocol

$ ping -6 localhost
PING localhost (::1) 56 bytes of data.
64 bytes from localhost (::1): icmp_seq=1 ttl=64 time=0.034 ms
64 bytes from localhost (::1): icmp_seq=2 ttl=64 time=0.024 ms
{% endhighlight %}

### How to Fix
* I'd tried to build latest version of `iputils` but not successfull :(
* So I'd revert it back to the older version

{% highlight bash %}
$ yum remove iputils
$ wget ftp://bo.mirror.garr.it/1/slc/centos/7.2.1511/os/x86_64/Packages/iputils-20121221-7.el7.x86_64.rpm
$ yum localinstall iputils-20121221-7.el7.x86_64.rpm
{% endhighlight %}

#### Check For Updates

{% highlight bash %}
$  yum update
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.fibergrid.in
 * epel: ftp.riken.jp
 * extras: mirror.fibergrid.in
 * updates: mirror.fibergrid.in
Resolving Dependencies
--> Running transaction check
---> Package iputils.x86_64 0:20121221-7.el7 will be updated
---> Package iputils.x86_64 0:20160308-8.el7 will be an update
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================================================================================
 Package                                 Arch                                   Version                                           Repository                            Size
=============================================================================================================================================================================
Updating:
 iputils                                 x86_64                                 20160308-8.el7                                    base                                 147 k

Transaction Summary
=============================================================================================================================================================================
Upgrade  1 Package

Total download size: 147 k
{% endhighlight %}

#### Blacklist iputils update

* I don't want iputils again updated next time
* So I'd blacklist `iputils` package from yum update

{% highlight bash %}
# Update the file as follows using the exclude keyword
$ vim /etc/yum.repos.d/CentOS-Base.repo
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
exclude=iputils
{% endhighlight %}

{% highlight bash %}
$ yum update
Loaded plugins: fastestmirror
base                                                                                                                                                  | 3.6 kB  00:00:00     
http://mirror.digistar.vn/centos/7.3.1611/extras/x86_64/repodata/repomd.xml: [Errno 12] Timeout on http://mirror.digistar.vn/centos/7.3.1611/extras/x86_64/repodata/repomd.xml: (28, 'Connection timed out after 30001 milliseconds')
Trying other mirror.
extras                                                                                                                                                | 3.4 kB  00:00:00     
updates                                                                                                                                               | 3.4 kB  00:00:00     
Loading mirror speeds from cached hostfile
 * base: mirror.fibergrid.in
 * epel: ftp.riken.jp
 * extras: mirror.fibergrid.in
 * updates: mirror.fibergrid.in
No packages marked for update
{% endhighlight %}
