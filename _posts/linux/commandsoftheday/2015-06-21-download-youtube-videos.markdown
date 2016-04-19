---
layout: post
title: "Download Youtube Videos"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "How to Download Youtube Videos with single command"
tags: [Linux, CommandLine, CommandOfTheDay, MAC OS X, Youtube]
image:
  feature:
date: 2015-06-21T12:44:35+05:30
---

{% include _toc.html %}

* `youtube-dl` is a small command-line program to download videos from YouTube.com and a few more sites.


### Requirement

* Python 2.6, 2.7 or 3.2+

### Install

#### On Linux Platform

##### Install youtube-dl by using curl command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo curl https://yt-dl.org/latest/youtube-dl -o /usr/local/bin/youtube-dl
[mitesh@Matrix ~]$ sudo chmod a+rx /usr/local/bin/youtube-dl
{% endhighlight %}

##### Install youtube-dl by using wget command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
[mitesh@Matrix ~]$ sudo chmod a+rx /usr/local/bin/youtube-dl
{% endhighlight %}

##### Install youtube-dl by using pip command
{% highlight bash %}
[mitesh@Matrix ~]$ sudo pip install youtube_dl
{% endhighlight %}

#### On Mac Platform

##### Install youtube-dl by using brew command
**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}
{% highlight bash %}
[mitesh@Matrix ~]$ brew install youtube-dl
{% endhighlight %}

#### On Windows Platform

* Download <a href="https://yt-dl.org/latest/youtube-dl.exe"> yotube-dl.exe </a>
* Place `youtube-dl.exe` in home directory or any other location on `PATH`.


**Refer:** <a href="https://github.com/rg3/youtube-dl">https://github.com/rg3/youtube-dl</a> for more information
