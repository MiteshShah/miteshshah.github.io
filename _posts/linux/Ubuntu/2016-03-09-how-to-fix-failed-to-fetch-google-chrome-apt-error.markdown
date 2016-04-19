---
layout: post
title: How to Fix 'Failed to Fetch' Google Chrome Apt Error
author:
modified:
comments: true
categories: linux/ubuntu
excerpt: "How to fix Google Chrome unable to find expected entry ‘main/binary-i386/Packages’ in Release file"
tags: [Linux, Ubuntu]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/13633091/119fc2a6-e613-11e5-8e1a-697afff7448d.jpg
  alt: How to Fix 'Failed to Fetch' Google Chrome Apt Error
  title: How to Fix 'Failed to Fetch' Google Chrome Apt Error
  feature:
date: 2016-03-09T11:56:44+05:30
---

{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">


* Google Chrome has pulled 32-bit Chrome builds from the official Chrome repo.
* which gets added to Ubuntu Software Sources when the app is installed first time.

* The ‘failed to fetch error that appears in the Terminal says:


{% highlight bash %}
“Failed to fetch http://dl.google.com/linux/chrome/deb/dists/stable/Release Unable to find expected entry ‘main/binary-i386/Packages’ in Release file (Wrong sources.list entry or malformed file)”
{% endhighlight %}

{% highlight bash %}
”Skipping acquire of configured file ‘main/binary-i386/Packages’ as repository ‘http://dl.google.com/linux/chrome/deb stable InRelease’ doesn’t support architecture ‘i386’”
{% endhighlight %}

### Fix Google Chrome Failed to Fetch apt-get error
{% highlight bash %}
# Add [arch=amd64] on following file
$ sudo vim /etc/apt/sources.list.d/google-chrome.list
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
{% endhighlight %}


{% highlight bash %}
$ sudo apt-get update
{% endhighlight %}
