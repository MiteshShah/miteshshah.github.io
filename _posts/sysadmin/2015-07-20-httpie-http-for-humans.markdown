---
layout: post
title: "HTTPie - HTTP for Humans"
author:
modified:
comments: true
categories: sysadmin
excerpt: "HTTPie - Command Line HTTP Client, Also Known for cURL and wget command Alternative."
tags: [Linux, SysAdmin, SystemAdmin, cURL, Wget, HTTPie]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/8771230/63389e92-2edc-11e5-8f35-8f96b4cec08f.png
  alt: HTTPie - cURL and wget Alternative
  title: HTTPie - cURL and wget Alternative
  feature:
date: 2015-07-20T12:35:44+05:30
---

{% include _toc.html %}

* HTTPie - Command Line HTTP Client.
* Also known for cURL and wget command Alternative.
* HTTPie goal is to make CLI interaction with web services as human-friendly as possible.

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

#### Features

* Expressive and intuitive syntax
* Formatted and colorized terminal output
* Built-in JSON support
* Forms and file uploads
* HTTPS, proxies, and authentication
* Arbitrary request data
* Custom headers
* Persistent sessions
* Wget-like downloads
* Python 2.6, 2.7 and 3.x support
* Linux, Mac OS X and Windows support
* Plugins
* Documentation
* Test coverage

#### Install HTTPie

##### Universal Installation

{% highlight bash %}
# Make sure we have an up-to-date version of pip and setuptools:
$ pip install --upgrade pip setuptools
$ pip install --upgrade httpie
{% endhighlight %}

##### Install on Debian Based Linux Distribution such as Debian/Ubuntu
{% highlight bash %}
$ sudo apt-get install httpie
{% endhighlight %}

##### Install on RPM Based Distribution such as REDHAT/CentOS
{% highlight bash %}
$ yum install httpie
{% endhighlight %}

#### Examples of HTTPie

* Use <a href="http://developer.github.com/v3/issues/comments/#create-a-comment">Github API</a> to post a comment on <a href="https://github.com/jkbrzt/httpie/issues/83#issuecomment-122190925">Github issue</a> with authentication

{% highlight bash %}
$ http -a USERNAME POST https://api.github.com/repos/jkbrzt/httpie/issues/83/comments body='HTTPie is awesome!'
{% endhighlight %}

* Upload file

{% highlight bash %}
$ http example.org < file.json
{% endhighlight %}

* Download file

{% highlight bash %}
$ http example.org/file > file

# wget style
$ http --download example.org/file
{% endhighlight %}

#### More Information

<iframe src="//www.slideshare.net/slideshow/embed_code/key/pa5H05w2QifByM" width="650" height="555" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/scottleber/htt-pie-minitalk" title="httpie" target="_blank">httpie</a> </strong> from <strong><a href="//www.slideshare.net/scottleber" target="_blank">Scott Leberknight</a></strong> </div>

* To get more detailed Information about HTTPie, Check <a href="http://httpie.org">http://httpie.org</a> website.
