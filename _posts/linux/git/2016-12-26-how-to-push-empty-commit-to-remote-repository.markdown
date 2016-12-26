---
layout: post
title: How to Push Empty Commit to Remote Repository
author:
modified:
comments: true
categories: linux/git
excerpt: "In this guide we can push empty commit on remote repository, Which allow us to setup the deployment process easily."
tags: [Linux, GIT, Github]
image:
  url:
  alt: How to Push Empty Commit to Remote Repository
  title: How to Push Empty Commit to Remote Repository
  feature:
date: 2016-12-26T15:50:58+05:30
---

* To setup deployment we need some commits on repository.
* But sometimes we need to setup deployment with empty repository
* In this guide, We will push empty commit message to remote repository, Which allow us to setup the deployment process.

{% highlight bash %}
$ git commit --allow-empty -m "Empty Commit to setup deployments"
$ git push
{% endhighlight %}
