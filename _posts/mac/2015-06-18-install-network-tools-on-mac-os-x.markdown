---
layout: post
title: "Install Network Tools on Mac OS X"
author:
modified:
comments: true
categories: mac
excerpt: "This article will teach you how to install network tools, such as nmap, mtr, nikto and dnsmap"
tags: [MAC OS X, Nmap, MTR, Nikto, Dnsmap]
image:
  feature:
date: 2015-06-18T16:02:50+05:30
---

{% include _toc.html %}

### Install Network Tools
* This article will teach you how to install network tools, such as
  * nmap
  * mtr
  * nikto
  * dnsmap

**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}

#### Install nmap

{% highlight bash %}
[mitesh@Matrix ~]$ brew install nmap
{% endhighlight %}

#### Install mtr
{% highlight bash %}
[mitesh@Matrix ~]$ brew install mtr
[mitesh@Matrix ~]$ sudo chown root:wheel /usr/local/Cellar/mtr/0.86/sbin/mtr
[mitesh@Matrix ~]$ sudo chmod u+s /usr/local/Cellar/mtr/0.86/sbin/mtr
{% endhighlight %}

* Add following line on your `~/.bashrc` or `~/.zshrc`
{% highlight bash %}
alias mtr=/usr/local/sbin/mtr
{% endhighlight %}

#### Install nikto
{% highlight bash %}
[mitesh@Matrix ~]$ brew install nikto
{% endhighlight %}

#### Install dnsmap
{% highlight bash %}
[mitesh@Matrix ~]$ brew install dnsmap
{% endhighlight %}

Feel free to edit this article or suggest missing tools.
