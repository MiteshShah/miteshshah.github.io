---
layout: post
title: "VIM - Automatic Open Readonly File With Sudo Privilege"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Most of time I'll open system files with vim and after edit the system file vim says It's readonly file."
tags: [Linux, CommandLine, CommandOfTheDay, VIM]
image:
  feature:
date: 2015-06-23T11:46:30+05:30
---

{% include _toc.html %}

* Most of time I'll open system files with vim and after edit the system file vim says It's readonly file.

### Method 1

* `:w !sudo tee %`
* This will ask to provide sudo password and save the file.

### Method 2

{% highlight bash %}
[mitesh@Matrix ~]$ echo "cmap w!! w !sudo tee > /dev/null %" >> ~/.vimrc
{% endhighlight %}

* Save readonly file with `:w!!`
* This will shortcut of Method 1.

### Method 3

#### Create Alias
{% highlight bash %}
[mitesh@Matrix ~]$ echo "alias vim='~/bin/autosudo.sh'" >> ~/.bashrc
{% endhighlight %}

#### Shell Script

* Save the below shell script as `~/bin/autosudo.sh`
{% highlight bash %}
#!/bin/bash
FILE=$1

# Check write permission
if [ -w $FILE ]
then
  /usr/bin/vim $FILE
else
  # Use sudo if we dont have write permissions
  sudo /usr/bin/vim $FILE
fi
{% endhighlight %}

#### Make Shell Script Executable

{% highlight bash %}
[mitesh@Matrix ~]$ chmod a+x ~/bin/autosudo.sh && source ~/.bashrc
{% endhighlight %}
