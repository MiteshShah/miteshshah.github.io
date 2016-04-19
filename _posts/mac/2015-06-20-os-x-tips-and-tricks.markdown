---
layout: post
title: "OS X Tips and Tricks"
author:
modified:
comments: true
categories: mac
excerpt: "All about OS X Hacks"
tags: [MAC OS X, Tips, Tricks]
image:
  feature:
date: 2015-06-20T14:21:52+05:30
---

{% include _toc.html %}

### General

#### Computer Name
{% highlight bash %}
# Get Computer Name
[mitesh@Matrix ~]$ scutil --get ComputerName
# Set Computer Name
[mitesh@Matrix ~]$ sudo scutil --set ComputerName "MiteshShah"
{% endhighlight %}

#### HostName
{% highlight bash %}
# Get HostName
[mitesh@Matrix ~]$ scutil --get HostName
# Set HostName
[mitesh@Matrix ~]$ sudo scutil --set HostName "MiteshShah"
{% endhighlight %}

#### LocalHostName
{% highlight bash %}
# Get LocalHostName
[mitesh@Matrix ~]$ scutil --get LocalHostName
# Set LocalHostName
[mitesh@Matrix ~]$ sudo scutil --set LocalHostName "MiteshShah"
{% endhighlight %}

#### Change NetBIOSName

{% highlight bash %}
# Get NetBIOSName
System Preferences > Network > Advanced > WINS
# Set NetBIOSName
[mitesh@Matrix ~]$ sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.smb.server NetBIOSName -string "MiteshShah"
{% endhighlight %}

#### Expand Save Panel
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode -bool true
{% endhighlight %}

### Finder

**NOTE!:** Restart Finder Application is required after making following changes.
{: .notice}

#### Finder Quite via ⌘ + Q
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder QuitMenuItem -bool true
{% endhighlight %}

#### Show Hidden Files
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder AppleShowAllFiles -bool true
{% endhighlight %}

#### Show Path Bar
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder ShowPathbar -bool true
{% endhighlight %}

#### Display POSIX Path in Windows Title
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
{% endhighlight %}

#### Disable Warning When Change File Extension
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false
{% endhighlight %}

#### Empty Trash without any warning
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder WarnOnEmptyTrash -bool false
{% endhighlight %}

#### Empty Trash Securely
{% highlight bash %}
[mitesh@Matrix ~]$ defaults write com.apple.finder EmptyTrashSecurely -bool true
{% endhighlight %}
