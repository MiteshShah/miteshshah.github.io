---
layout: post
title: How to Add Some Colors and Emoji Icons on CommandLine/Terminal
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "How to customize CommandLine/Terminal and add some Rainbow Colors and Emoji icons on that."
tags: [Linux, MAC, CLI, CommandLine]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9191101/c42caec4-401a-11e5-8444-77f2b7ca8c59.png
  alt: How to Add Some Colors and Emoji Icons on Command Line/terminal
  title: How to Add Some Colors and Emoji Icons on Command Line/terminal
  feature:
date: 2015-08-11T11:17:10+05:30
---

{% include _toc.html %}

### How to add some Rainbow Colors on your CommandLine/Terminal

{% highlight bash %}
$ gem install lolcat
{% endhighlight %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### How to add some Emoji Icons on your CommandLine/Terminal

{% highlight bash %}
$ sudo sh -c "curl https://raw.githubusercontent.com/mrowa44/emojify/master/emojify -o /usr/local/bin/emojify && chmod +x /usr/local/bin/emojify"
{% endhighlight %}

<img alt="Add Emoji Icon on CommandLine/Terminal" src="https://raw.githubusercontent.com/mrowa44/emojify/master/img/after.png">
