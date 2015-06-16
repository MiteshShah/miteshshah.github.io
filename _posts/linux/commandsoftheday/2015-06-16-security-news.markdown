---
layout: post
title: "Security News"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Get latest Security News on your Terminal"
tags: [Linux, CommandLine, CommandOfTheDay]
image:
  feature: https://cloud.githubusercontent.com/assets/1223371/8183107/4d1d7626-1453-11e5-9bbf-8c9597dc026c.png
date: 2015-06-16T17:04:43+05:30
---

### Security News

* Get latest security news on your Terminal.
* Just add following line on your `~/.bashrc` or `~/.zshrc`
{% highlight bash %}
dig +short -t txt istheinternetonfire.com
"Logjam! Weak Diffie-Hellman downgrade attack on TLS https://weakdh.org/ https://weakdh.org/imperfect-forward-secrecy.pdf"
{% endhighlight %}
