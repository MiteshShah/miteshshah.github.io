---
layout: post
title: "How to Push Specific Commit With Git"
author:
modified:
comments: true
categories: linux/git
excerpt: "Sometimes we made lots of commits locally and want to push specific commit to the remote branch. I'll show you how to push specific/single commit on remote branch"
tags: [Linux, GIT, Github]
image:
  feature:
date: 2015-06-26T17:16:59+05:30
---

* Sometimes we made lots of commits locally and want to push specific commit to the remote branch.
* I'll show you how to push specific commit on remote branch.

* First we need to find out the hash of commit which we want to push on remote.
{% highlight bash %}
[mitesh@Matrix ~]$ git log --pretty=oneline
de7e2a469a28d4d74fd4597369cebabd0832636a Test commit for new post
f61b48cb8b1877721e2596a6aa65648a68bb605e new post
{% endhighlight %}


**Syntax:**
{% highlight bash %}
git push <remote name> <commit hash>:<remote branch name>
{% endhighlight %}

* Now I want to push only `f61b48cb8b1877721e2596a6aa65648a68bb605e` commit

{% highlight bash %}
# Git push only one commit
[mitesh@Matrix ~]$ git push origin f61b48cb8b1877721e2596a6aa65648a68bb605e:master  
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (9/9), done.
Writing objects: 100% (10/10), 1.36 KiB | 0 bytes/s, done.
Total 10 (delta 5), reused 0 (delta 0)
To git@github.com:MiteshShah/miteshshah.github.io.git
   b02924a..f61b48c  f61b48cb8b1877721e2596a6aa65648a68bb605e -> master
{% endhighlight %}

**Note!**: This will push all commits up to and including the specified commit!
{: .notice}
