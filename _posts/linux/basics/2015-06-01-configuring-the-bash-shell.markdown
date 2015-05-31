---
layout: post
title: "Configuring the Bash Shell"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - How to configure BASH Shell"
tags: [Linux, Basics, BASH]
image:
  feature:
date: 2015-06-01T10:00:00+05:30
---

{% include _toc.html %}


### Variables

* A variable is a label that equates some value.
* The value can change over the time, across the accounts or across the systems but the label remains constant.

**For Example:**

* A shell script may place a file in `$HOME`, a reference to the value of the variable `HOME`.
* This value may differ, depending on who is running the shell script.

**NOTE!:** Some variables, like `$HOME`,
Are intended to be referenced but not set by the user.
{: .notice}

* However, users can also create their own variables:

**For Examples:**

{% highlight bash %}
[mitesh@Matrix ~]$ HI="Hello, pleased to meet you."
[mitesh@Matrix ~]$ echo $HI
Hello, pleased to meet you.
{% endhighlight %}

#### Types of Variables

##### Local(Shell) Variable
* Bash variables are local to a single shell By Default.
* Set with `VARIABLE=VALUE`

##### Environment Variable
* Accessed by some programs for configuration
* Set with `export VARIABLE=VALUE`


* The `set`, `env`, and `echo` commands can be used to display all variables, environment variables, and a single variable value, respectively.

**For Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ set | less
[mitesh@Matrix ~]$ env | less
[mitesh@Matrix ~]$ echo $HOME
/home/mitesh
{% endhighlight %}

#### Common Variables
<pre>
/-------------------------------------------------------------------------------------------------------\
|													|
|	PS1		|	Appearance of the bash prompt						|
|	PATH		|	Directories to look for executables in					|
|	EDITOR		|	Default text editor							|
|	LS_COLORS	|	Display different file types in different colors			|
|	HISTFILESIZE	|	Maximum number of commands saved in a user's .bash_history file		|
|													|
\-------------------------------------------------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ echo $PS1
[\u@\h \W]\$
{% endhighlight %}

#### Common Escape Sequences
<pre>
/----------------------------------------------------------------------\
|	\u	|	User Name					|
|	\h	|	Short Hostname (Not FQDN)			|
|	\w	|	The Current Working Directory			|
|	\!	|	The History Number Of This Command		|
|	\$	|	If The Effective UID Is 0, a #, Otherwise a $	|
\-----------------------------------------------------------------------/
</pre>

**NOTE!:** For complete list of escape sequence, see the PROMPTING section of the bash man page.
{: .notice}
<br>

#### Information Variables
<pre>
/-----------------------------------------------\
|						|
|	HOME	|	User's Home Directory	|
|	EUID	|	User's Effective UID	|
|						|
\-----------------------------------------------/
</pre>

#### Aliases

* Aliases are shortcut names for longer commands.
* Use alias by itself to see all set aliases
* Use alias followed by alias name to see alias value
* Aliases can be used for security purposes to force you to use certain flags


**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ alias
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias vi='vim'

[mitesh@Matrix ~]$ alias l.
alias l.='ls -d .* --color=auto'
{% endhighlight %}


**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ alias rm="rm -i"
{% endhighlight %}

**NOTE!:**	In this case, If you ever want to use the rm command itself, instead of your alias.
You can precede the command with a blackslash (\).
{: .notice}
{% highlight bash %}
[mitesh@Matrix ~]$ \rm -r Junk
{% endhighlight %}

**NOTE!:** The alias value must be a single word and so you will almost always want to quote the value as shown. <br>
Aliases are local to a single shell By Default.
{: .notice}


### How Bash Expand a Command Line

1. Split The Line Into Words
2. Expand Aliases
3. Expand Curly-Brace Statements ({})
4. Expand Tilde Statements (~)
5. Expand Variables ($) And Command-Substitution ($() and ``)
6. Split The Line Into Words Again
7. Expand File Globs (*, ?, [abc], etc)
8. Prepare I/O Redirections (<, >)
9. Run The Command!

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ ll ~/*.{o,c} >> $HOME/co-files-$(date -I)
[mitesh@Matrix ~]$ ls -l --color=auto /home/mitesh/hello.c /home/mitesh/hello.o >> /home/mitesh/co-files-2009-09-03
{% endhighlight %}

#### Preventing Expansion

**Backslash (\\) Makes The Next Characters Literal**
{% highlight bash %}
[mitesh@Matrix ~]$ echo Your Cost: \$5.00
Your Cost: $5.00
{% endhighlight %}

**Quoting Prevents Expansion**

* `'` (Single Quotes) - Inhibit All Expansion
* `"` (Double Quotes) - Inhibit All Expansion Except
* `\` (Backslash)		-	Single Character Inhibition
* `$` (Doller Sign)	-	Variable Expansion
* `\`` (Back Quotes)	-	Command Substitution
* `!` (Exclamation Point)	-	History Substitution


**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ echo 'Welcome To $(hostname)'
Welcome To $(hostname)

[mitesh@Matrix ~]$ echo "Welcome To $(hostname)"
Welcome To Matrix

[mitesh@Matrix ~]$ echo \*\*\* SALE \*\*\*
*** SALE ***

[mitesh@Matrix ~]$ echo '*** SALE ***'
*** SALE ***
{% endhighlight %}

### Login vs Non-login Shells

* Startup Is Configured Differently For Login and Non-login Shells
<pre>
/---------------------------------------------------------------\
|	Login Shell		|	Non-login Shell		|
|----------------------------------------------------------------
|								|
|	/etc/profile		|	~/.bashrc		|
|	/etc/profile.d/		|	/etc/bashrc		|
|				|	/etc/profile.d/		|
|	~/.bash_profile		|				|
|	~/.bashrc		|				|
|	/etc/bashrc		|				|
|								|
\---------------------------------------------------------------/
</pre>

**Login Shell**

* `su -` Any Shell Created At Login (Includes X Login)

**Non-login Shell**

* su
* Executed Script
* Graphical Terminals
* Any Other Bash Instance


### Bash Startup Tasks

* By Default, Most of the configuration items,
That we have seen in this unit are only valid for the given shell.
* But Typically, We want settings to be established every time we start a shell,
Rather than typing all of our variables, aliases, and other commands on a per shell basis.
* To accomplish this, we use startup scripts, scripts that runs when a shell is created.

**profile**

* Stored in `/etc/profile` (Global) and `~/.bash_profile` (User)
* Runs For Login Shells Only
* Used For
  * Running Commands (Example: Mail Checker Script)
  * Setting Environment Variables (Such As PATH, USER, LOGNAME, MAIL, HOSTNAME, HISTSIZE, HISTCONTROL)

**bashrc**

* Stored in `/etc/bashrc` (Global) and `~/.bashrc` (User)
* Runs For All The Shells
* Used For
  * Setting umask
  * Defining/Undefining Aliases
  * Setting Local Variables (Such As PS1)

### Bash Exit Tasks

**logout**

* Stored in `~/.bash_logout` (User)
* Runs when Login Shell Exits
* Used For
  * Creating Automatic Backup
  * Cleaning Out Temporary Files

### Sourcing Files

* Changes made to profile and bashrc files need to be sourced.
* This will execute the file and read it into the running shell.

* For Instance, If you make a changes to `/etc/bashrc`.
Opening a new terminal will source this file and activate your changes.
However, The original terminal where you made those changes still has old settings.
From the original terminal, Run one of the following command
{% highlight bash %}
[root@Matrix ~]# . /etc/bashrc
[root@Matrix ~]# source /etc/bashrc
{% endhighlight %}
