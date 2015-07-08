---
layout: post
title: "Useful Bash Commands"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "Most useful Linux/Unix Commands, A collection of commands which can save lot's of our typing time."
tags: [Linux, CommandLine, CommandOfTheDay, Bash]
image:
  feature:
date: 2015-07-07T13:16:28+05:30
---


{% include _toc.html %}

**NOTE!**: If you feel some commands are missing in this post,<br>
Feel free to add those commands in below comments box.
{: .notice}

#### Shortcuts
<pre>
/---------------------------------------------------------------------------------------\
|	!!	Repeat last command							|
|	!char	Repeat last command That started with char				|
|	!num	Repeat a command by its number in history output			|
|										        |
|	!-n	Repeat a command entered n command back					|
|	!?abc	Repeat last command that contains (as opposed to ?started with?) abc    |
\---------------------------------------------------------------------------------------/
</pre>
<pre>
/---------------------------------------------------------------\
|	UP DOWN Keys	Scroll through previous commands	|
|	Ctrl+r		Reverse-i-search		  	|
\---------------------------------------------------------------/
</pre>

#### Clear Terminal
{% highlight bash %}
# Linux/Unix OS
$ <CTRL+l>
# MAC OS
$ <COMMAND+k>
{% endhighlight %}

#### Run Previous Command With Root/Sudo Privilege
{% highlight bash %}
$ sudo !!
{% endhighlight %}

#### Run Previous Command With Search/Replace For First Instance
{% highlight bash %}
$ echo "Hello Mitesh, Hello Shah, Hello Visitor"
Hello Mitesh, Hello Shah, Hello Visitor
$ ^Hello^Hi
$ echo "Hi Mitesh, Hello Shah, Hello Visitor"
Hi Mitesh, Hello Shah, Hello Visitor
{% endhighlight %}

#### Run Previous Command With Search/Replace For All The Instance
{% highlight bash %}
$ echo "Hello Mitesh, Hello Shah, Hello Visitor"
Hello Mitesh, Hello Shah, Hello Visitor
$ !!:gs/Hello/Hi
$ echo "Hi Mitesh, Hi Shah, Hi Visitor"
Hi Mitesh, Hi Shah, Hi Visitor
{% endhighlight %}

#### Execute Command Without Saving in History

* Prepending one or more spaces to your command won't be saved in history.
{% highlight bash %}
$ <SPACE><COMMAND>
{% endhighlight %}

**NOTE!**: `HISTCONTROL=ignorespace` will ignore just the commands that begin with a space.<br>
Use `HISTCONTROL=ignoreboth` if you also want to ignore duplicates.
{: .notice}

#### Paste Last Argument of Most Recent Command
<pre>
/-----------------------------------------------------------------------\
|	Esc .		The escape key followed by a period.		|
|	Alt+.		Hold down alt key while pressing the period.	|
|	!$		Only valid for last argument			|
\-----------------------------------------------------------------------/
</pre>

{% highlight bash %}
$ cp -v index.html /home/mitesh/shah/miteshshah.github.io/index.html
# Using ALT Dot
$ cd <ALT+.>
$ cd /home/mitesh/shah/miteshshah.github.io/index.html
# Using Esc Dot
$ cd <ESC+.>
$ cd /home/mitesh/shah/miteshshah.github.io/index.html
# Using !$
$ cd !$
$ cd /home/mitesh/shah/miteshshah.github.io/index.html
{% endhighlight %}

#### Reset Terminal

* Sometime we send binary output to the terminal, Which make terminal not useable.
* The `reset` command reset your terminal, Note that when you type reset its won't properly echo back on terminal.
{% highlight bash %}
$ reset
{% endhighlight %}

#### Display Mount Filesystem in Nice Layout
{% highlight bash %}
$ mount | column -t
/dev/root  on  /                         type  ext4         (rw,relatime,errors=remount-ro,data=ordered)
devtmpfs   on  /dev                      type  devtmpfs     (rw,relatime,size=4037132k,nr_inodes=1009283,mode=755)
none       on  /dev/pts                  type  devpts       (rw,nosuid,noexec,relatime,mode=600)
none       on  /proc                     type  proc         (rw,nosuid,nodev,noexec,relatime)
none       on  /sys                      type  sysfs        (rw,nosuid,nodev,noexec,relatime)
none       on  /proc/sys/fs/binfmt_misc  type  binfmt_misc  (rw,nosuid,nodev,noexec,relatime)
none       on  /sys/fs/fuse/connections  type  fusectl      (rw,relatime)
none       on  /sys/kernel/security      type  securityfs   (rw,relatime)
none       on  /run                      type  tmpfs        (rw,nosuid,noexec,relatime,size=807520k,mode=755)
none       on  /run/lock                 type  tmpfs        (rw,nosuid,nodev,noexec,relatime,size=5120k)
none       on  /run/shm                  type  tmpfs        (rw,nosuid,nodev,relatime)
{% endhighlight %}

#### ASCII Table
{% highlight bash %}
$ man ascii
{% endhighlight %}

#### Compare Remote File with Local File
{% highlight bash %}
$ ssh USERNAME@HOSTNAME cat /path/to/remotefile | diff /path/to/localfile -
{% endhighlight %}


#### Decide Which Command Run On Success and Fail
{% highlight bash %}
$ cal && echo "Right Command" || echo "Wrong Command"
July 2015
Su Mo Tu We Th Fr Sa  
     1  2  3  4  
5  6  7  8  9 10 11  
12 13 14 15 16 17 18  
19 20 21 22 23 24 25  
26 27 28 29 30 31

Right Command

$  call && echo "Right Command" || echo "Wrong Command"
No command 'call' found, did you mean:
 Command 'wall' from package 'bsdutils' (main)
 Command 'calc' from package 'apcalc' (universe)
 Command 'cal' from package 'bsdmainutils' (main)
call: command not found

Wrong Command
{% endhighlight %}

#### Execute Command After Every 2 Seconds

{% highlight bash %}
$ watch -d df -h
Every 2.0s: df -h                                                                                                                      Tue Jul  7 13:52:39 2015

Filesystem      Size  Used Avail Use% Mounted on
rootfs          500G  250G  250G  50% /
/dev/root       500G  250G  250G  50% /
devtmpfs        3.9G  4.0K  3.9G   1% /dev
none            789M  165M  625M  21% /run
none            5.0M     0  5.0M   0% /run/lock
none            3.9G     0  3.9G   0% /run/shm
{% endhighlight %}

### Check Last Command Status

* If command run Successfully then exit status is zero
* If command doesn't run Successfully then exit status is non-zero
{% highlight bash %}
$ cal ; echo $?
$ call ; echo $?
{% endhighlight %}
