---
layout: post
title: "How to Change Bash Custom Prompt (PS1)"
author:
modified:
comments:
categories: linux/commandsoftheday
excerpt: "How to change bash custom prompt and make it more usable. Think when you enter wrong command your bash prompt turn into RED."
tags: [Linux, CommandLine, CommandOfTheDay, PS1]
image:
  feature:
date: 2015-06-25T16:33:54+05:30
---

{% include _toc.html %}

* Most of our work on shell prompt.
* By Default, `PS1` is little bit boring.
* I'll show you how to add useful information on your shell prompt

### Alert Prompt (PS1)

* Alert Prompt (PS1) change your shell color to RED when your previous command exit status is non zero.

{% highlight bash %}
[root@miteshshah.github.io ~]# PS1="\`if [ \$? = 0 ]; then echo \[\e[34m\]^_^[\u@\h:\w]\\$\[\e[0m\]; else echo \[\e[31m\]O_O[\u@\h:\w]\\$\[\e[0m\]; fi\` "
{% endhighlight %}

<img src="https://cloud.githubusercontent.com/assets/1223371/8353379/d71ee752-1b5a-11e5-9017-fd449fc57b73.png">

### Display Date and Time
{% highlight bash %}
[root@miteshshah.github.io ~]# PS1='\[\033[0;32m\]┌──\[\033[0;36m\]\u@\h\[\033[0m\033[0;32m\]─┤├─ \[\033[0m\]\t \d\[\033[0;32m\] ─┤├─ \[\033[0;31m\]\w\[\033[0;32m\] ─┤ \n\[\033[0;32m\]└──▶ \$\[\033[0m\] '
┌──root@miteshshah─┤├─ 11:18:43 Thu Jun 25 ─┤├─ ~ ─┤
└──▶ #
{% endhighlight %}

### Generate Prompt Online

* By using <a href="http://bashrcgenerator.com/"> http://bashrcgenerator.com/</a> you can generate your prompt easily.

**NOTE!**: For more detailed information use `man bash` and search for PROMPTING.
{: .notice}
