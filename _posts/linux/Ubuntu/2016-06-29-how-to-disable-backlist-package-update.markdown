---
layout: post
title: How to Disable/Backlist Package Update
author:
modified:
comments: true
categories: linux/ubuntu
excerpt: "Step by step guide to disable/block or blacklist package updates using APT"
tags: [Linux, Ubuntu, Debian, APT, DPKG]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/16448424/05f35b6c-3e0f-11e6-93aa-9ab02b8bc650.png
  alt: How to Disable/Backlist Package Update
  title: How to Disable/Backlist Package Update
  feature:
date: 2016-06-29T15:21:08+05:30
---

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Disable/Backlist Package Update

The `apt-mark` command will mark or unmark a software package as being automatically installed and it is used with option `hold` or `unhold`.

* **hold** – this option used to mark a package as held back, which will block the package from being installed, upgraded or removed.

* **unhold** – this option used to remove a previously set hold on a package and allow to install, upgrade and remove package.

{% highlight bash %}
# Lock PHP Packages
$ sudo apt-mark hold php*

# Unlock PHP Packages
$ sudo apt-mark unhold php*
{% endhighlight %}
