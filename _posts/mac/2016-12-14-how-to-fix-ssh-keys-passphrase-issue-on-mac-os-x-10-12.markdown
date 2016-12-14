---
layout: post
title: How to Fix SSH Keys/passphrase Issue on Mac OS X 10.12
author:
modified:
comments: true
categories: mac
excerpt: "How to fix SSH Agent issue on MAC OS X Sierra with auto startup script file"
tags: [MAC OS X, Sierra, SSH]
image:
  url:
  alt: How to Fix SSH Keys/passphrase Issue on Mac OS X 10.12
  title: How to Fix SSH Keys/passphrase Issue on Mac OS X 10.12
  feature:
date: 2016-12-14T15:30:52+05:30
---


{% include _toc.html %}

### Summary

* In previous versions of MAC OS, `ssh-agent` used to remember the passphrase for the keys I added to the keychain with `ssh-add -K`.
* After a reboot (or logout/login), it automatically picked up the passphrase from the keychain with no extra step.

* In MAC OS X 10.12 Sierra, I have to manually poke the agent to recognize there are passphrase on the keychain

#### How to Fix

{% highlight bash %}
$ wget -O ~/Library/LaunchAgents/ssh_add.plist https://raw.githubusercontent.com/MiteshShah/admin/master/Mac/ssh_add.plist
{% endhighlight %}

* Above command automatically run `ssh-add -A` at login/reboot.
