---
layout: post
title: "VIM - Automatic Open Readonly File With Sudo Privilege"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Most of time I'll open system files with vim and after edit the system file vim says It's readonly file."
tags: [Linux, commandsoftheday, VIM]
image:
  feature:
date: 2015-06-23T11:46:30+05:30
---

* Most of time I'll open system files with vim and after edit the system file vim says It's readonly file.
* The following script make my life easy.

### Create Alias
{% highlight bash %}
[mitesh@Matrix ~]$ echo "alias vim='~/bin/autosudo.sh'" >> ~/.bashrc
{% endhighlight %}

### Shell Script

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

### Make Shell Script Executable

{% highlight bash %}
[mitesh@Matrix ~]$ chmod a+x ~/bin/autosudo.sh && source ~/.bashrc
{% endhighlight %}


**NOTE!** You can also use `:w !sudo tee %` in case you open readonly file and want to save it.
{: .notice}
