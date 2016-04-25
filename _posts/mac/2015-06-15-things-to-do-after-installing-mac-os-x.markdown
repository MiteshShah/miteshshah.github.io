---
layout: post
title: "Things to Do After Installing Mac OS X"
author:
modified: 2015-07-06T11:15:12+05:30
comments: true
categories: mac
excerpt: "Newbie Guide - Install Git, OpenSSH-Server, Java, Shutter, Hipchat, VLC and Google Chrome on Mac OS"
tags: [MAC OS X, Yosemite, El Captain, Homebrew]
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

#### Install ssh-copy-id command
{% highlight bash %}
[mitesh@Matrix ~]$ brew install ssh-copy-id
{% endhighlight %}

#### Install wget command
{% highlight bash %}
[mitesh@Matrix ~]$ brew install wget
{% endhighlight %}

#### Install watch command
{% highlight bash %}
[mitesh@Matrix ~]$ brew install watch
{% endhighlight %}

#### Install Google Chrome
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install google-chrome
[mitesh@Matrix ~]$ brew cask install 1password
{% endhighlight %}

#### Install VLC
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install vlc
{% endhighlight %}

#### Install Skitch
* See something that sparks an idea? Use Skitch to snap it, mark it up, and send it on in an instant.
* Your bold ideas stand out even brighter with Skitch.

{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install skitch
{% endhighlight %}

#### Install Caffeine

* Caffeine is a tiny program that puts an icon in the right side of your menu bar.
* Click it to prevent your Mac from automatically going to sleep, dimming the screen or starting screen savers.
* Click it again to go back.
* Right-click (or ⌘-click) the icon to show the menu.

{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install caffeine
{% endhighlight %}

#### Install Atom

* A hackable text editor for the 21st Century.
* Atom is a text editor that's modern, approachable, yet hackable to the core—a tool you can customize to do anything but also use productively without ever touching a config file.

{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install atom
{% endhighlight %}

#### Install Vagrant
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install vagrant
{% endhighlight %}

#### Install Virtualbox
{% highlight bash %}
[mitesh@Matrix ~]$ brew cask install virtualbox
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
[mitesh@Matrix ~]$ curl https://raw.githubusercontent.com/MiteshShah/admin/master/Mac/macosx.sh | bash
{% endhighlight %}


The installation may take a little time, so grab a cup of coffee <i class="fa fa-coffee"></i> and relax.
