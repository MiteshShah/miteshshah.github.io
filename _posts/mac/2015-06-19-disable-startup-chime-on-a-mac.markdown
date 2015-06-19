---
layout: post
title: "Disable Startup Chime on a Mac"
author:
modified:
comments: true
categories: mac
excerpt: "Each time you start a Mac, you'll hear a familiar start-up chime. I'll show you how to  mute the Mac start up chime completely."
tags: [MAC, Startup, Tips, Tricks]
image:
  feature:
date: 2015-06-19T17:45:45+05:30
---

#### Mute Startup Chime
{% highlight bash %}
[mitesh@Matrix ~]$ sudo nvram SystemAudioVolume=%80﻿
{% endhighlight %}

### Enable Startup Chime
* In future if you change your mind and decided that it is best to have the start up chime

{% highlight bash %}
[mitesh@Matrix ~]$ sudo nvram -d SystemAudioVolume
{% endhighlight %}

* This will restore the start up chime.
