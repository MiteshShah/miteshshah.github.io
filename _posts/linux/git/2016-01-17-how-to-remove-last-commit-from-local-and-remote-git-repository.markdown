---
layout: post
title: How to Remove Last Commit From Local & Remote Git Repository
author:
modified:
comments: true
categories: linux/git
excerpt: "Sometimes by mistake we push commits on remote branch. I'll show you how to remove/delete last commit from remote branch"
tags: [Linux, GIT, Github]
image:
  url:
  alt: How to Remove Last Commit From Local & Remote Git Repository
  title: How to Remove Last Commit From Local & Remote Git Repository
  feature:
date: 2016-01-17T16:33:33+05:30
---

* Sometimes by mistake we push commits on remote branch.
* I'll show you how to remove/delete last commit from local & remote branch.

{% highlight bash %}
[mitesh@shah ~]$ git log --pretty=oneline
037d772391792ff85b394e1e43f5f04bb7515e11 Add PrintFriendly Button
8f2cc3d5b585076d6001117703d677e24f07247f Minor Update on Resume
9e57e45fa58caef1eb95d804997bb5f70708d8c8 HTTPS version in Author Bio
{% endhighlight %}


* Now I want to delete last commit `037d772391792ff85b394e1e43f5f04bb7515e11`

{% highlight bash %}
[mitesh@shah ~]$ git reset --hard HEAD^
HEAD is now at 8f2cc3d Minor Update on Resume

[mitesh@shah ~]$ git push -f
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:MiteshShah/miteshshah.github.io.git
 + 037d772...8f2cc3d master -> master (forced update)
{% endhighlight %}
