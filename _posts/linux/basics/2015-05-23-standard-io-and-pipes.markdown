---
layout: post
title: "Standard IO & Pipes"
comments: true
modified:
categories: linux/basics
excerpt: "Newbie Guide - Standard Input/Output & Pipes"
tags: [Linux, Linux Basics, Standard IO, Pipes]
image:
  feature:
date: 2015-05-23T23:20:24+05:30
---
{% include _toc.html %}

### Standard Input and Output

* Linux provides three I/O channels to the programs.
  1. Standard Input  (STDIN),	File Descriptor Number 0,	By Default Keyboard
  2. Standard Output (STDOUT),	File Descriptor Number 1,	By Default Screen or Terminal Window
  3. Standard Error  (STDERR),	File Descriptor Number 2,	By Default Screen or Terminal Window

**Syntax:**
{% highlight bash %}
command operator filename
{% endhighlight %}



#### Supported Operators
* \>	Redirect STDOUT to File
* 2>	Redirect STDERR to File
* &>	Redirect All Output (STDOUT and STDERR)

**NOTE!:** File contents are overwritten by default. Use >\> to appends.
{: .notice}
<br>

### Redirecting Output to File

* STDOUT and STDERR can be redirected to files.

**Examples:**
<pre>
* command < file                Send file as a Input to the command.
* command > file		Redirect STDOUT of command to file.
* command >> file		Append STDOUT of command to file.
* command 2> file		Redirect STDERR of command to file.
* command 2>> file    	        Append STDERR of command to file.
</pre>



Run the following command as a non-root user
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd
find: `/etc/dhcp': Permission denied
find: `/etc/lvm': Permission denied
/etc/passwd
...output truncated...
{% endhighlight %}

Note what happens when the same command is run but STDOUT is redirected to a file
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd > find.out
find: `/etc/dhcp': Permission denied
...output truncated...

[mitesh@Matrix ~]$ cat find.out
/etc/passwd
/etc/pam.d/passwd
{% endhighlight %}

**NOTE!:** The STDOUT and STDERR are distinct, Redirecting one does not affect the other.
{: .notice}

<br>
The following command would redirect STDERR to a file
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd 2> find.err
/etc/passwd
/etc/pam.d/passwd
{% endhighlight %}

* The above command only show the STDOUT of the command.
* If you really do not care about the errors then why you waste a file (find.err)?
* There is a special file on your system that is very useful in this sort of situation, `/dev/null`.
* `/dev/null` is a black hole for data. Anything sent to is simply ignored.


#### Display only STDOUT
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd 2> /dev/null
/etc/passwd
/etc/pam.d/passwd
{% endhighlight %}

#### Store STDOUT but ignore STDERR
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd > find.out 2> /dev/null
{% endhighlight %}

Guess what happen when run the following command
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd > find.out 2> find.err
[mitesh@Matrix ~]$ find /etc -name passwd >> find.out 2>> find.err
{% endhighlight %}


### Redirecting Output to Program
* Linux and UNIX provides many small utilities that perform one task very well.
* A core design feature of Linux and UNIX is that the output of one command can be fed directly as a input for another command.
* Pipes can connect the commands.
{% highlight bash %}
command1 | command2
command1 | command2 | command3...
{% endhighlight %}

**NOTE!:** STDERR is not forwarded across the pipes.
{: .notice}
<br>
**Examples:**
{% highlight bash %}
[mitesh@Matrix man]$ pwd
/usr/share/man

[mitesh@Matrix man]$ ls -C | tr 'a-z' 'A-Z'
MAN1 MAN2 MAN3 MAN4 MAN5 MAN6 MAN7 MAN8 MAN9 MAIN PT_BR TMAC.H WHATIS
{% endhighlight %}

In the above example, All the lower case letters are converted to upper case letters
<br><br>
**less - View input one page at a time**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l /etc | less
Note: Input can be searched with /
{% endhighlight %}

**mail - Send input via email**
{% highlight bash %}
[mitesh@Matrix ~]$ echo "Test Email" | mail -s "Testing Mail" user@example.com
{% endhighlight %}

**lpr - Send input to a printer**
{% highlight bash %}
[mitesh@Matrix ~]$ echo "Test Printer" | lpr
[mitesh@Matrix ~]$ echo "Test Printer" | lpr -P printer_name
{% endhighlight %}



### Redirecting All Output

* Some operators affect both STDOUT and STDERR
* `&>` Redirect All Output (STDOUT and STDERR)
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd &> find.all
{% endhighlight %}

#### Redirecting I/O Channels to Each Other

* Run the following command as a non-root user:
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd | less
{% endhighlight %}

* You will find that while STDOUT is display through less, STDERR is not.
* This is because a pipe only redirect STDOUT.
* If you wanted to send all output to less you would needed to redirect STDERR to STDOUT first.
* You can redirect one I/O channel to another using `>` and the channel's file descriptor numbers.

**For Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd 2>1
{% endhighlight %}
* The above command simply redirect all STDERR to a file called 1 rather than to file descriptor 1 (STDOUT).
* To tell your shell that you are referring to a file descriptor, prepend the `&`

**For Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ find /etc -name passwd 2>&1 | less
{% endhighlight %}

### Combining Outputs

* Suppose you wanted to run two command back to back and send their output through the pipe.

**For Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ cal 2014; cal 2015 | lpr
{% endhighlight %}
* The output of these commands, you would find that only the calendar for 2015 was printed, while the calendar for 2014 went to the screen

* This can be overcome by running the cal command in a subshell.
{% highlight bash %}
[mitesh@Matrix ~]$ (cal 2011; cal 2012) | lpr
{% endhighlight %}

### Redirecting to Multiple Target

**Syntax:**
{% highlight bash %}
command1 | tee filename | command2
{% endhighlight %}
* Stores STDOUT of command1 in filename, then pipes to command2

**Uses:**

* Troubleshooting complex pipelines.
* Simultaneous viewing and logging of output

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -lR /etc | tee stage1.out | sort | tee stage2.out | uniq -c | tee stage3.out | sort -r | tee stage4.out | less
[mitesh@Matrix work]$ generate_report.sh | tee report.txt
{% endhighlight %}

### Redirecting STDIN from a file
* Redirect STDIN with `<`

**Examples:**
{% highlight bash %}
[mitesh@Matrix work]$ tr 'A-Z' 'a-z' < .bash_profile
[mitesh@Matrix work]$ cat .bash_profile | tr 'A-Z' 'a-z'

[mitesh@Matrix work]$ cat filename.txt
Hello, World!
[mitesh@Matrix work]$ cat < filename.txt
Hello, World!
{% endhighlight %}

### Sending Multiple Lines to STDIN
* Redirect Multiple line from keyboard to STDIN with <\<WORD
* All text untill WORD is sent to STDIN.
* Sometimes called heretext

**Example:**
{% highlight bash %}
[mitesh@Matrix work]$ mail -s "Please call" jane@example.com <<END
> Hi Jane,
>
> Please give me a call when you get in. We may need to do some maintenance on server1.
>
> Details when you're on-site,
> Boris
> END
{% endhighlight %}

### For Loop - Shell Scripting
* Performs actions on each member of a set of values.

**Syntax:**
{% highlight bash %}
for variable in value1 value2...
do
  command using $variable
done
{% endhighlight %}

**Examples:**
{% highlight bash %}
for NAME in root neo mitesh
do
  ADDRESS="$NAME@localhost"
  MESSAGE='Projects are due today!'
  echo $MESSAGE | mail-s Reminder $ADDRESS
done
{% endhighlight %}

{% highlight bash %}
for USER in $(grep bash /etc/passwd)...
for FILE in *.txt...

for num in {1..10}
for num in $(seq 1 10)
for num in $(seq 0 10 100)
{% endhighlight %}

{% highlight bash %}
#!/bin/bash
# alive2.sh
# Check to see if hosts 192.168.0.1-192.168.0.255 are alive

for n in {1..255}
do
  host=192.168.0.$n
  ping -c2 $host &> /dev/null

  if [ $? = 0 ]; then
    echo "$host is UP"
  else
    echo "$host is DOWN"
  fi
done
{% endhighlight %}
