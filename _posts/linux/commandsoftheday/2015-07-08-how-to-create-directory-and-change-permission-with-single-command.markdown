---
layout: post
title: "How to Create Directory and Change Permission With Single Command"
author:
modified: 2015-07-10T11:45:02+05:30
comments: true
categories: linux/commandsoftheday
excerpt: "Quiz - How to create new directory and change its Permission with single command"
tags: [Linux, CommandLine, CommandOfTheDay, Bash, MKDIR]
image:
  feature:
date: 2015-07-08T21:20:02+05:30
---

{% include _toc.html %}

### Command Line Quiz

* Let's check our command line knowledge
* To solve this quiz you have to tell single command (In below comments box) which can create new directory and change its Permission to 600.

### Rules

* No `&&` is allowed

{% highlight bash %}
$ mkdir quiz && chmod 600 quiz
{% endhighlight %}

* No `;`  is allowed
{% highlight bash %}
$ mkdir quiz ; chmod 600 quiz
{% endhighlight %}

**NOTE!**: Answer of this quiz will be updated on 10th July 2015.
{: .notice}

### Answer
{% highlight bash %}
$ mkdir -m 600 quiz
$ ls -ld quiz/
drw------- 2 root root 4096 Jul 10 11:36 quiz/
{% endhighlight %}
