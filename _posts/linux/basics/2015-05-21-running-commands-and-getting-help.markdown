---
layout: post
title: "Running Commands and Getting Help"
comments: true
modified: 2015-05-21T15:20:50+05:30
categories: linux/basics
excerpt: "Newbie Guide - How to run commands & get help on Linux"
tags: [Linux, Basics, Tutorials, Commands]
image:
  feature:
date: 2015-05-21T11:20:50+05:30
---
{% include _toc.html %}

### Running Commands

* Each item is separated by a space.
* Commands have the following syntax <br>
  `command options arguments`
  1. Options modify the command's behavior
  * Single letter options usually preceded by - <br>
Can be passed as -a -b -c or -abc
  * Full word options usually preceded by -\- <br>
Example: `--help`

  1. Arguments are file names or other data needed by the command

* Multiple commands can be separated by semicolon (;)
  `mkdir backups; cp *.txt backups/``


### Some Simple Commands

#### Date - Display date and time

* date command prints system date and time.
* The format is configurable via an optional formatting string (see options with `date --help`)
{% highlight bash %}
[mitesh@Matrix ~]$ date
Fri Aug 12 10:05:48 IST 2011
[mitesh@Matrix ~]$ date +"Today is %A, %B %d, %Y. %nIt is %r, %z."
Today is Thursday, May 21, 2015.
It is 11:47:47 AM, +0530
{% endhighlight %}

#### Cal  - Display Calendar

* cal command prints an ASCII calendar of the current month.
* When single numeric argument is passed to the cal command, cal command prints the given year calendar.
* For example: `cal 06` will print the calendar for the year 0006, not the year 2006.
* When a month and year is passed as argument to the cal command, cal command prints the particular month calendar.
{% highlight bash %}
[mitesh@Matrix ~]$ cal 09 2011
[mitesh@Matrix ~]$ cal 06; cal -3
[mitesh@Matrix ~]$ cal 2006; cal -j
[mitesh@Matrix ~]$ cal 09 2011;	cal -3 09 2011
[mitesh@Matrix ~]$ cal 09 1752;	cal -j 10 2011
{% endhighlight %}


### Getting Help

* Don't try to memorise everythings!
* Many levels of help
1. whatis command
1. help option
1. man command
1. info command
1. Extended Documentation


#### 1. whatis comand

* Display short discriptions of commands.
* whatis command uses a database that is updated nightly, so often not available immediately after install.
* Generate whatis database without waiting for automatic update, run `makewhatis` command as root user.
{% highlight bash %}
[mitesh@Matrix ~]$ whatis cal
cal    (1)   - displays a calendar
cal    (1p)  - print a calendar
{% endhighlight %}

#### 2. -\-help option

* Just knowing what a command does is not always enough.
* In order to use a command effectively you need to know what options and arguments it accepts (the syntax of the command).
* Most commands have a -\-help options, but not all the commands have -\-help options.
* The command -\-help option Display usage summary and argument list.
{% highlight bash %}
[mitesh@Matrix ~]$ date --help
Usage: date [OPTION]... [+FORMAT] or:
date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
Display the current time in the given FORMAT, or set the system date.
...argument list omitted...
{% endhighlight %}

##### Reading Usage Summaries

* Arguments in [] are optional.
* Arguments in <> or CAPS are variables.
* Text followed by ... represent a list.
* x\|y\|z means x or y or z.
* -abc means any mix of -a, -b or -c.

#### 3. The man command:

* Almost every commands as well as most of the configuration files and several developer's libraries on Linux system has an associated man pages, which provides more through documentations than the command `--help` options.
* The man pages normally contains the following sections
1. NAME
2. SYNOPSIS
3. DISCRIPTION
4. OPTIONS
5. FILES
6. BUGS
7. EXAMPLES
8. SEE ALSO

* The collection of all the man pages on a system is called Linux Manual.
* The Linux Manual is divided into several sections, each of which covers a particular topics, and every man page is associated with exactly one of these sections.
* The Linux Manual sections are:
{% highlight yaml %}
1 User commands   4 Special files   7 Miscellaneous
2 System calls    5 File formats    8 Administrative commands
3 Library calls   6 Games
{% endhighlight %}
* Often, Linux commands, calls, and files are referenced by a name followed by manual section number in parentheses.
{% highlight bash %}
passwd(1) - which is accessed by running man 1 passwd, refers to the user command passwd.
passwd(5) - which is accessed by running man 5 passwd, refers to the file format for /etc/passwd
{% endhighlight %}


##### Navigating man pages

* Navigate with arrows, PgUp, PgDn
* /text searches for text
* n/N goes to next/previous match
* q quits man page


##### Searching Linux Manual

* Suppose you do not know the name of command you are looking for, in this case you can perform keyword search.
* To perform keyword search its uses whatis database.

{% highlight bash %}
man -f keyword (List all man pages that contains keyword in the command name)
man -k keyword (List all man pages that contains keyword in the command name or its short discriptions)
man -K keyword (List all man pages that contains keyword in the command name or its short discriptions or anywhere else)
{% endhighlight %}

#### 4. The info command

* Similar to man command, but often more in-depth.
* Run info command without arguments, list all the info pages.
* The structure of info pages are looks like a website.
* Each page is divided into nodes.
* Links to nodes are precededby *.
* info [command]


##### Navigating info pages

* Navigate with arrows, PgUp, PgDn
* Tab moves to next link
* Enter follows the selected link
* s text searches for text (default: last search)
* n/p /u/l  goes to next/previous/up-one/last node
* q quits info page

**NOTE!:** If you prefer the navigation keys used by man commands, such as / n and N, you can start info command with --vi-keys argument.
Example: info --vi-keys [command]
{: .notice}


#### 5. Extended Documentation

* The /usr/share/doc directory contains the documentations that does not fit the length or the format of a man and info pages.
* The /usr/share/doc is a location of documents that does not fit elsewhere.
* Sample configuration files, tutorials
* HTML/PDF/PS Documentation
* License Details
