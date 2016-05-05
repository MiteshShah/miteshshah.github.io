---
layout: post
title: "Bash & Shell Scripting"
comments: true
modified:
categories: linux/basics
excerpt: "Newbie Guide - BASH & Shell Scripting"
tags: [Linux, Linux Basics, Bash, ShellScripting]
image:
  feature:
date: 2015-05-23T20:57:28+05:30
---

{% include _toc.html %}

### Command Line Shortcuts

#### File Globbing
* Globbing is a wildcard expansion.
* `*` matches zero or more characters.
* `?` matches any single characters.
* `[0-9]` matches a range of numbers.
* `[abc]` matches any of the characters in the list.
* `[^abc]` matches all except the characters in the list.
* Predefined character classes can be used.
* The syntax for a character classes is `[:keyword:]`, where keyword can be - alpha, upper, lower, digit, alnum, punct, space.

**Examples:**
{% highlight bash %}
[mitesh@Matrix work]$ ls
123.doc  abc.txt  file1  file name.txt  jkL.txt
456.doc  Def.txt  file2  Ghi.txt        MnO.txt

[mitesh@Matrix work]$ echo *.txt
abc.txt Def.txt file name.txt Ghi.txt jkL.txt MnO.txt

[mitesh@Matrix work]$ echo *.doc
123.doc 456.doc

[mitesh@Matrix work]$ echo [[:upper:]]*
Def.txt Ghi.txt MnO.txt

[mitesh@Matrix work]$ echo ??[[:upper:]]*
jkL.txt MnO.txt

[mitesh@Matrix work]$ echo *[[:space:]]*
file name.txt

[mitesh@Matrix work]$ echo [[:digit:]]*
123.doc 456.doc

[mitesh@Matrix work]$ echo [^[:digit:]]*
abc.txt Def.txt file1 file2 file name.txt Ghi.txt jkL.txt MnO.txt

[mitesh@Matrix work]$ echo [[:lower:]]?[[[:upper:]]*
jkL.txt
{% endhighlight %}

#### The Tab Key

* Type Tab to complete command lines
* For the command name, it will complete a command name.
* For an argument, it will complete a file name.

**Examples:**
{% highlight bash %}
[mitesh@Matrix work]$ ls
dove  eagle  myfile.txt  pelican  penguin
{% endhighlight %}

<pre>
/---------------------------------------------------------------------------------------\
|	Input   |     1st Tab           |       2nd Tab 				|
|---------------------------------------------------------------------------------------|
|               |                       |   Display all commands                        |
|	ca      |		        |   List all commands that start with ca	|
|	dat     |	date            |	                                	|
|---------------------------------------------------------------------------------------|
|	cat d   |	cat dove        |                                               |
|	cat m   |	cat myfile.txt  |			                        |
|	cat p   |	cat pe          |   List all possible file names		|
\---------------------------------------------------------------------------------------/
</pre>

#### History
* Bash stores a history of commands you've entered.
* Use `history` command to see list of REMEMBERED commands.

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

#### To recall last argument from previous command
<pre>
/-----------------------------------------------------------------------\
|	Esc .		The escape key followed by a period.		|
|	Alt+.		Hold down alt key while pressing the period.	|
|	!$		Only valid for last argument			|
\-----------------------------------------------------------------------/
</pre>

* Use `^old^new` to repeat the last command with old changed to new.

{% highlight bash %}
[mitesh@Matrix ~]$ cp filter.c /usr/local/src/project
[mitesh@Matrix ~]$ ^filter^frontend
cp frontend.c /usr/local/src/project
{% endhighlight %}

* You can ignore repeated duplicate commands and repeated lines that only differ in prepended spaces by running the following command below, or by adding it to your `.bashrc` file.

{% highlight bash %}
[mitesh@Matrix ~]$ export HISTCONTROL=ignoreboth
{% endhighlight %}

**NOTE!**: `HISTCONTROL=ignorespace` will ignore just the commands that begin with a space.<br>
Use `HISTCONTROL=ignoreboth` if you also want to ignore duplicates.
{: .notice}


### Command Line Expansion

#### The Tilde

* Tilde (~) refers to home directory.
* Tilde (~) is useful in environment where home directories exist in non-standard locations.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ cat ~/.bash_history
[mitesh@Matrix ~]$ cat ~neo/.bash_profile
[mitesh@Matrix ~]$ ls ~julie/public_html
{% endhighlight %}

#### Command Substitution
* Use of the backquotes is called command substitution.
* An alternative syntax of backquotes is to place the command in parentheses preceded by a dollar sign `$()`.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ echo "The system name is `hostname`"
The system name is Matrix
[mitesh@Matrix ~]$ echo "The system name is $(hostname)"
The system name is Matrix
{% endhighlight %}

#### Brace Expansion
* Shorthand for printing repetitive strings.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ echo file{1,2,3}
file1 file2 file3

[mitesh@Matrix ~]$ echo file{1..5}
file1 file2 file3 file4 file5

[mitesh@Matrix ~]$ rm -rf file{1..5}

[mitesh@Matrix ~]$ mkdir -p work/{inbox,outbox,pending}/{normal,urgent,important}
{% endhighlight %}


### Bash Variables

* Variables are named values useful for storing data or commands output.
* Set with
`VARIABLE=VALUE`
* Referenced with `$VARIABLE`

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ HI="Hello, and welcome to $(hostname)"
[mitesh@Matrix ~]$ echo $HI
Hello, and welcome to Matrix

[mitesh@Matrix ~]$ FILES=$(ls /etc)
[mitesh@Matrix ~]$ echo $FILES
abrt acpi adjtime aliases aliases.db alsa alternatives anacrontab anthy-conf asound.conf at.deny audisp audit autofs_ldap_auth.conf
...output truncated...
{% endhighlight %}

**NOTES!:** Bash recognizes that you are trying to set a variable when it sees the pattern `text=text`. Note that there can be no spaces on either side of the = or you will get an error.
{: .notice}

### Command Editing Tricks
<pre>
/---------------------------------------------------------------\
|	Ctrl+a		Moves to beginning of the line		|
|	Ctrl+e		Moves to end of the line		|
|	Ctrl+u		Deletes to beginning of the line	|
|	Ctrl+k		Deletes to end of the line		|
|	Ctrl+Arrow	Moves left or right by word		|
\---------------------------------------------------------------/
</pre>

### Gnome Terminal

* Accessed via Applications -> Accessories -> Terminal
* Graphical Terminal supports multiple tabbed shells.
<pre>
/-----------------------------------------------------------------------\
|	Ctrl+Shift+t		Creates a new tab			|
|	Ctrl+Shift+c		Copies selected text			|
|	Ctrl+Shift+v		Paste text				|
|	Ctrl+PgUp/Pgdn		Switches to next/previous tab		|
|	Shift+PgUp/PgDn		Scroll up and down a screen at a time	|
\-----------------------------------------------------------------------/
</pre>

### Scripting Basics

* A shell script is simply a text file containing a series of commands or statements to be executed.
* Shell scripts are useful for
  1. Automating commonly used commands.
  2. Performing a system administration and troubleshooting.
  3. Creating simple applications.
  4. Manipulation of text or files.

#### Creating Shell Script
* Comments start with a `#`
* First line contains the Magic Shebang Sequence `#!`
* This tells the operating system which interpreter to use in order to execute the script.
{% highlight bash %}
#!/bin/bash
#!/bin/sh
#!/bin/csh
#!/usr/bin/perl
#!/usr/bin/python
{% endhighlight %}

* Make the script executables
{% highlight bash %}
[mitesh@Matrix ~]$ chmod u+x myscript.sh
[mitesh@Matrix ~]$ chmod 750 myscript.sh
{% endhighlight %}

* Ensure that the script is located in a directory listed by the `PATH` environmental variable.
* To do this enter the following command
{% highlight bash %}
[mitesh@Matrix ~]$ echo $PATH
/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/mitesh/bin
{% endhighlight %}

* If the script is not in a directory listed in the `PATH` variable, either move the script to a directory that is (such as `$HOME/bin`) or specify the absolute or relative path on the command line when executing the script

{% highlight bash %}
[mitesh@Matrix ~]$ /home/mitesh/mytestscript.sh
[mitesh@Matrix ~]$ cd /home/mitesh
[mitesh@Matrix ~]$ ./mytestscript.sh
{% endhighlight %}

#### First Shell Script
{% highlight bash %}
#!/bin/bash
# This script displays some information about your environment


echo "Greetings. The date and time are $(date)"
echo "Your working directory is: $(pwd)"

[mitesh@Matrix ~]$ chmod u+x info.sh
[mitesh@Matrix ~]$ ls -l info.sh
-rwxrw-r--. 1 mitesh mitesh 165 May 23 20:00 info.sh

[mitesh@Matrix ~]$ ./info.sh
[mitesh@Matrix ~]$ ~/info.sh
[mitesh@Matrix ~]$ /home/mitesh/info.sh
Greetings. The date and time are Sat May 23 20:02:20 IST 2015
Your working directory is: /home/mitesh
{% endhighlight %}
