---
layout: post
title: SSH Tips and Tricks
author:
modified:
comments: true
categories: linux/SSH
excerpt: "Simplify Your Life With an SSH Config File"
tags: [Linux, OpenSSH, SSH]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-25T16:19:05+05:30
---

{% include _toc.html %}

### Logging In

{% highlight bash %}
$ ssh user@example.com
{% endhighlight %}

* And, if needed, we can specify a different port

{% highlight bash %}
$ ssh user@example.com -p 3307
{% endhighlight %}

* AWS Loggin In

{% highlight bash %}
$ ssh -i /path/to/identity.pem user@example.com
{% endhighlight %}

**NOTE!** You may need to set permissions on the `.pem` file so only the owner can read/write/execute it. <br>
If you don't aware of how to set/change Permission <a href="/linux/basics/users-groups-and-permissions/#changing-permissions"> Check this guide</a>
{: .notice}

### SSH Config

* If you're anything like me, you probably log in and out of a half dozen remote servers (or these days, local virtual machines) on a daily basis.
* And if you're even <b>more like me</b>, you have trouble remembering all of the various usernames, remote addresses and command line options for things like specifying a non-standard connection port.
* Here's something really powerful.

{% highlight bash %}
$ vim ~/.ssh/config
Host srv001
    HostName example.com
    Port 2222
    User MiteshShah
    IdentityFile  ~/.ssh/id_example
    IdentitiesOnly yes

Host srv002
    HostName 192.168.33.10
    User root
    PubkeyAuthentication no

Host aws
    HostName some.address.ec2.aws.com
    User ubuntu
    IdentityFile  ~/.ssh/aws_identity.pem
    IdentitiesOnly yes
{% endhighlight %}

#### Let's cover the options used above:

* `HostName` - The server host (domain or ipaddress)
* `Port` - The port to use when connecting
* `User` - The username to log in with
* `IdentityFile` - The SSH key identity to use to log in with, if using SSH key access
* `IdentitiesOnly` - `Yes` to specify only attempting to log in via SSH key
* `PubkeyAuthentication` - `No` to specify you wish to bypass attempting SSH key authentication

### Let's Loggin In

* No need to remember Username/IdentityFile/Port etc

{% highlight bash %}
$ ssh aws
{% endhighlight %}
