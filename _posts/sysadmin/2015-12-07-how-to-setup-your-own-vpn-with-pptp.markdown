---
layout: post
title: How to Setup Your Own VPN With PPTP
author:
modified:
comments: true
categories: sysadmin
excerpt: "A Point-To-Point Tunneling Protocol (PPTP) allows you to implement your own VPN very quickly, and is compatible with most mobile devices. Even though PPTP is less secure than OpenVPN, it is also faster and uses less CPU resources."
tags: [SysAdmin]
image:
  url:
  alt: How to Setup Your Own VPN With PPTP
  title: How to Setup Your Own VPN With PPTP
  feature:
date: 2015-12-07T14:48:57+05:30
---


{% include _toc.html %}

### Setup PPTP Server

#### PPTP Installation
{% highlight bash %}
# Ubuntu
$ sudo apt-get install pptpd
# CentOS
$ sudo yum -y install epel-release
$ sudo yum -y install pptpd
{% endhighlight %}

##### Setup IP Address
* Now you should edit `/etc/pptpd.conf` and add the following lines
{% highlight bash %}
$ vim /etc/pptpd.conf
localip 10.0.0.1
remoteip 10.0.0.100-200
{% endhighlight %}

**NOTE:** Where localip is IP address of your server and remoteip are IPs that will be assigned to clients that connect to it.
{: .notice}

##### Setup Authentication for PPTP
{% highlight bash %}
$ vim /etc/ppp/chap-secrets
# Secrets for authentication using CHAP
# client	server	secret			IP addresses
MiteshShah	pptpd	MyPassword		*
{% endhighlight %}

**NOTE:** Where client is the username, server is type of service – pptpd for our example, secret is the password, and IP addresses specifies which IP address may authenticate.<br>
By setting `*` in IP addresses field, you specify that you would accept username/password pair for any IP.
{: .notice}

##### Add DNS Server
{% highlight bash %}
$ vim /etc/ppp/options.pptpd
ms-dns 8.8.8.8
ms-dns 8.8.4.4
{% endhighlight %}

##### Restart PPTP Service
{% highlight bash %}
$ sudo service pptpd restart
{% endhighlight %}


#### Setup Forwarding
* It is important to enable IP forwarding on your PPTP server.
* This will allow you to forward packets between public IP and private IPs that you setup with PPTP.

{% highlight bash %}
$ sudo vim  /etc/sysctl.conf
net.ipv4.ip_forward = 1

$ sudo sysctl -p
{% endhighlight %}

#### Create a NAT rule for iptables

{% highlight bash %}
$ sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE && iptables-save

# If you would also like your PPTP clients to talk to each other, add the following iptables rules
$ sudo iptables --table nat --append POSTROUTING --out-interface ppp0 -j MASQUERADE
$ sudo iptables -I INPUT -s 10.0.0.0/8 -i ppp0 -j ACCEPT
$ sudo iptables --append FORWARD --in-interface eth0 -j ACCEPT
{% endhighlight %}

### Setup PPTP Clients

* Install PPTP Client

{% highlight bash %}
$ sudo apt-get install network-manager-pptp network-manager-pptp-gnome pptp-linux
{% endhighlight %}

* Open Network Connections, VPN tab, click on Add button.
* Select Point-to-Point Tunneling Protocol (PPTP) from the list then click Create button

<img width="496" alt="PPTP VPN Setup" src="https://cloud.githubusercontent.com/assets/1223371/11624593/f29f553a-9cfa-11e5-98d8-3c676f615421.png">

* On the Gateway type the PPTP Server public IP Address

<img width="424" alt="PPTP VPN Setup" src="https://cloud.githubusercontent.com/assets/1223371/11624594/f2cd611e-9cfa-11e5-9c34-c1999d4833fc.png">

* Click on the Advanced button to open PPTP advanced Options.
* On the Security and Compression check ONLY Use Point-to-Point Encryption (MPPE), Security: All available (Default), allow Deflate data compression, Use TCP header compression.

<img width="332" alt="PPTP Advance Options" src="https://cloud.githubusercontent.com/assets/1223371/11624749/d6a7ad5e-9cfb-11e5-9f9a-cb4984b67df3.png">


### Troubleshooting PPTP

#### Fix GRE Protocol

**Issue:**

* If you are behind firewall/squid you may be face GRE Protocol errors

{% highlight bash %}
Dec  7 05:08:38 li724-160 pptpd[25197]: GRE: read(fd=7,buffer=60a400,len=8260) from network failed: status = -1 error = Protocol not available
Dec  7 05:37:28 li724-160 pptpd[2253]: GRE: read(fd=7,buffer=60a400,len=8260) from network failed: status = -1 error = Protocol not available
Dec  7 05:37:28 li724-160 pptpd[2253]: CTRL: GRE read or PTY write failed (gre,pty)=(7,6)
{% endhighlight %}

**FIX**

* Add necessary Kernel module
{% highlight bash %}
$ sudo modprobe ip_gre
$ sudo modprobe ip_nat_pptp
$ sudo modprobe ppp_mppe
$ sudo modprobe ip_conntrack_pptp
{% endhighlight %}
