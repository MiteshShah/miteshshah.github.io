---
layout: post
title: "Things to Do After Installing Mac OS X"
author:
modified:
comments: true
categories: mac
excerpt: "Newbie Guide - Install Git, OpenSSH-Server, Java, Shutter, Hipchat, VLC and Google Chrome"
tags: [MAC, Yosemite, ]
image:
  feature:
date: 2015-06-15T14:54:12+05:30
---

{% include _toc.html %}

### Manual Way to Install Software

#### Install HomeBrew
{% highlight bash %}
[mitesh@Matrix ~]$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

#### Install HomeBrew  Cash
{% highlight bash %}
[mitesh@Matrix ~]$ brew install caskroom/cask/brew-cask
{% endhighlight %}

#### Install Google Chrome
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install google-chrome
[mitesh@Matrix ~]$ brew cask install 1password
{% endhighlight %}

#### Install Vagrant
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install vagrant
{% endhighlight %}

#### Install Virtualbox
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install virtualbox
{% endhighlight %}

#### Install VLC
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install vlc
{% endhighlight %}

#### Install Atom
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install atom
{% endhighlight %}

#### Install watch command
{% highlight bash %}
[mitesh@Matrix ~]$ brew install watch
{% endhighlight %}

#### Install Oh-My-ZSH
{% highlight bash %}
[mitesh@Matrix ~]$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

# Change your default shell
[mitesh@Matrix ~]$ chsh -s /bin/zsh
{% endhighlight %}

### Automate Above Install Steps

* All above install steps can be automate in following shell scripts.

{% highlight bash %}
[mitesh@Matrix ~]$ wget -c https://raw.githubusercontent.com/MiteshShah/admin/master/Mac/macosx.sh
[mitesh@Matrix ~]$ bash macosx.sh
{% endhighlight %}


The installation may take a little time, so grab a cup of coffee <i class="fa fa-coffee"></i> and relax.
