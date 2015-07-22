---
layout: post
title: "WARNING: Remote Host Identification has Changed"
author:
modified:
comments: true
categories: linux/tutorials
excerpt: "How to fix Remote Host Identification has Changed issue."
tags: [Linux, OpenSSH]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/8822056/2e6dacac-3080-11e5-9147-cfb24b70a2b5.png
  alt: WARNING Remote Host Identification has Changed
  title: WARNING Remote Host Identification has Changed
  feature:
date: 2015-07-22T14:33:21+05:30
---

* Mostly after change the server or change ssh keys we get following message
{% highlight bash %}
$ ssh mitesh@example.com
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
4e:24:e3:a6:83:da:48:ae:11:f3:5d:95:69:03:d3:cb.
Please contact your system administrator.
Add correct host key in /Users/Mitesh/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/Mitesh/.ssh/known_hosts:37
RSA host key for example.com has changed and you have requested strict checking.
Host key verification failed.
{% endhighlight %}

**How to Fix:**

{% highlight bash %}
$ ssh-keygen -R example.com
# Host example.com found: line 37 type RSA
/Users/Mitesh/.ssh/known_hosts updated.
Original contents retained as /Users/Mitesh/.ssh/known_hosts.old
{% endhighlight %}
