---
layout: post
title: "Assign Multiple IP Address"
author:
modified:
comments:
categories: sysadmin
excerpt: "How to Assign Multiple IP Address to Single LAN Card using IP Aliasing"
tags: [Linux, SysAdmin, Network, NIC]
image:
  feature:
date: 2015-06-21T18:08:23+05:30
---

{% include _toc.html %}

* Assign Multiple IP Address to Single LAN Card is called IP Aliasing
* IP Aliasing is useful for create virtual websites on Apache or Nginx.
* The main advantage of IP Alias is, you don't have to purchase new LAN Card for each ip address.

### Setup Network Alias

* Let’s assume that we want to attach two ip address (192.168.0.10 & 192.168.0.11) to the eth0 NIC card.
<pre>
/-----------------------------------------\
| Adaptor     IP Address        Type      |
|------------------------------------------
| eth0        192.168.0.9       Primary   |
| eth0:0      192.168.0.10      Alias 1   |
| eth0:1      192.168.0.11      Alias 2   |
\-----------------------------------------/
</pre>

#### Temporary Setup
{% highlight bash %}
[mitesh@Matrix ~]$ sudo ifconfigeth0:0 192.168.0.10 up
[mitesh@Matrix ~]$ sudo ifconfigeth0:1 192.168.0.11 up
{% endhighlight %}

**NOTE!:** Above chnages lost when you restart system or reload/restart network service.
{: .notice}

#### Permantly Setup

#### RedHat/CentOS

* Let's copy the Primary NIC configuration file.
{% highlight bash %}
[mitesh@Matrix ~]$ cd /etc/sysconfig/network-scripts/
[mitesh@Matrix ~]$ cp -v ifcfg-eth0 ifcfg-eth0:0
[mitesh@Matrix ~]$ cp -v ifcfg-eth0 ifcfg-eth0:1
{% endhighlight %}

* Open `ifcfg-eth0` and view the contents.
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO=static
ONBOOT=yes
TYPE="Ethernet"
IPADDR=192.168.0.9
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
HWADDR=00:0B:42:28:AB:4B
{% endhighlight %}

* We have to modify DEVICE and IPADDR parameter for our virtual network interface (eth0:0 & eth0:1)

{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/sysconfig/network-scripts/ifcfg-eth0:0
DEVICE="eth0:0"
BOOTPROTO=static
ONBOOT=yes
TYPE="Ethernet"
IPADDR=192.168.0.10
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
HWADDR=00:0B:42:28:AB:4B

[mitesh@Matrix ~]$ cat /etc/sysconfig/network-scripts/ifcfg-eth0:1
DEVICE="eth0:1"
BOOTPROTO=static
ONBOOT=yes
TYPE="Ethernet"
IPADDR=192.168.0.11
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
HWADDR=00:0B:42:28:AB:4B
{% endhighlight %}

#### Debian/Ubuntu

* Open `/etc/network/interfaces` file and view the contents.
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/network/interfaces
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug eth0
#iface eth0 inet dhcp
iface eth0 inet static
	address  192.168.0.9
	netmask	 255.255.255.0
	gateway  192.168.0.1
	dns-nameservers 8.8.8.8 8.8.4.4
{% endhighlight %}

* We have to add following line to setup virtual network interface (eth0:0 & eth0:1)
{% highlight bash %}
# Virtual Aliases
auto eth0:0
iface eth0:0 inet static
        address 192.168.0.10
        netmask 255.255.255.0

auto eth0:1
iface eth0:1 inet static
        address 192.168.0.11
        netmask 255.255.255.0
{% endhighlight %}

**NOTE!:** Once, you’ve made all changes, save all your changes and restart the network service for the changes to reflect.
{: .notice}

### Verify Network Alias
{% highlight bash %}
[mitesh@Matrix ~]$ ifconfig
eth0      Link encap:Ethernet  HWaddr 00:0B:42:28:AB:4B
          inet addr:192.168.0.9  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe80::96de:80ff:fe63:ab6a/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:198507112 errors:0 dropped:0 overruns:0 frame:0
          TX packets:187207576 errors:0 dropped:0 overruns:0 carrier:1
          collisions:0 txqueuelen:1000
          RX bytes:96441149361 (89.8 GiB)  TX bytes:59909542838 (55.7 GiB)
          Interrupt:18

eth0:0    Link encap:Ethernet  HWaddr 00:0B:42:28:AB:4B
          inet addr:192.168.0.10  Bcast:192.168.0.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          Interrupt:18 Base address:0x2000

eth0:1    Link encap:Ethernet  HWaddr 00:0B:42:28:AB:4B
          inet addr:192.168.0.11  Bcast:192.168.0.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          Interrupt:18 Base address:0x2000
{% endhighlight %}
