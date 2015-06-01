---
layout: post
title: "Finding and Processing Files"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about locate and find command"
tags: [Linux, Basics, Find, Locate]
image:
  feature:
date: 2015-06-01T14:10:40+05:30
---

{% include _toc.html %}

### locate command
* Locate - Find Files by Name
* Locate reads one or more databases prepared by `updatedb` command.


#### Options
<pre>
/----------------------------------------------------------------------------------------------\
|												|
|	-n x		|	List only the 1st X matches					|
|	-i, ignore-case	|	Ignore case distinctions when matching patterns.		|
|	-b, basename	|	Match  only  the base name against the specified patterns.	|
|												|
\-----------------------------------------------------------------------------------------------/
</pre>

#### Examples
{% highlight bash %}
# Search boot in File Name or Path
[mitesh@Matrix ~]$ locate boot
/boot
/boot/grub
/boot/grub/grub.conf
/etc/pam.d/reboot
...output truncated...

# Search boot in File Name (Basename)
[mitesh@Matrix ~]$ locate -b boot
/boot
/etc/pam.d/reboot
/etc/rc.d/init.d/firstboot
/sbin/reboot
/usr/bin/reboot
...output truncated...
{% endhighlight %}

### find command
* Find - Searches Directory Trees In Real Time
* Slower But More Accurate Than Locate
* CWD Is Used If No Starting Directory Is Given
* All Files Are Matched If No Criteria Is Given


**Examples:**
{% highlight bash %}
# Find would only return the files that were named .png,
# Not files that contained in their name the string .png
[mitesh@Matrix ~]$ find -name .png
.png

# Fortunately, You can use shell wild cards with find, But must be quoted
[mitesh@Matrix ~]$ find -name "*.png"
Mahavir.png	Ganesha.png	Hello.png	.png

# Search for files named snow.png in the current working directory
[mitesh@Matrix ~]$ find -name snow.png

# Serach for files named snow.png Snow.png SNOW.PNG etc in the CWD
[mitesh@Matrix ~]$ find -iname snow.png

# Search files anywhere on the system that ends with .txt
[mitesh@Matrix ~]$ find / -name "*.txt"

# Search files in the /etc directory that contain pass in their names
[mitesh@Matrix ~]$ find /etc -name "*pass*"

# Search files owned by the user joe and the group joe in the /home directory
[mitesh@Matrix ~]$ find /home -user joe -group joe

# Search files owned by UID 500 and the GID 600 in the /home directory
[mitesh@Matrix ~]$ find /home -uid 500 -gid 600
{% endhighlight %}

#### Logical Operators
* If multiple criteria are given to the find commands:
* All criteria are ANDed together by default.
* This behavior can be overridden with the -or or -not(!) options.

##### Operator Precedence
* \\( Expression... \\)
* The Logical Not(-not, !)
* The Logical And(-and, -a)
* The Logical Or(-or, -o)

**NOTE!:** \\( Expression... \\)	Force Precedence, Include space after the \\( and before the \\)
{: .notice }
<br>

**Examples:**
{% highlight bash %}
# Search files anywhere on the system that ends with .png AND owned by the user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -a -user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -and -user mitesh

# Search files anywhere on the system that ends with .png OR owned by the user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -o -user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -or -user mitesh

# Search files anywhere on the system that ends with .png and NOT  owned by the user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" ! -user mitesh
[mitesh@Matrix ~]$ find / -name "*.png" -not -user mitesh
{% endhighlight %}

##### Force Precedence
{% highlight bash %}
# List all the files that is not owned by the user mitesh
[mitesh@Matrix ~]$ find -not \( -user mitesh \)

# List all the files that is not owned by the user mitesh or neo
[mitesh@Matrix ~]$ find -not \( -user mitesh -o -user neo \)
{% endhighlight %}

#### Find And Permissions
<pre>
/-----------------------------------------------------------------------------------------------------\
|													|
|	-perm  mode	|	File’s permission bits are exactly same as the mode.			|
|	-perm -mode	|	File's permission bits are atleast contain the mode + Extra Mode	|
|	-perm /mode	|	Any of the permission bits mode are set for the file.			|
|													|
\-----------------------------------------------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix find]$ ls -l
total 0
-rw-rw-r--. 1 mitesh mitesh 0 Sep  5 15:26 file1
-rw-rw-r--. 1 mitesh mitesh 0 Sep  5 15:26 file2
-rwxrw-r--. 1 mitesh mitesh 0 Sep  5 15:26 file3
-r--rwxrw-. 1 mitesh mitesh 0 Sep  5 15:26 file4
-rwxrwxrwx. 1 mitesh mitesh 0 Sep  5 15:26 file5
-rw-r-----. 1 mitesh mitesh 0 Sep  5 15:26 file6
-r--r-----. 1 mitesh mitesh 0 Sep  5 15:26 file7
-r--r--r--. 1 mitesh mitesh 0 Sep  5 15:26 file8
----------. 1 mitesh mitesh 0 Sep  5 15:26 file9

[mitesh@Matrix find]$ find -perm 664			[mitesh@Matrix find]$ find -perm 764
./file1							./file3
./file2

[mitesh@Matrix find]$ find -perm -664			[mitesh@Matrix find]$ find -perm -222
./file1							./file5
./file2
./file3							[mitesh@Matrix find]$ find -perm -755
./file5							./file5

[mitesh@Matrix find]$ find -perm /444			[mitesh@Matrix find]$ find -perm -002
./file1							./file4
./file2							./file5
./file3
./file4
./file5							[mitesh@Matrix find]$ find -perm -764
./file6							./file3
./file7							./file5
./file8

[mitesh@Matrix find]$ find -perm /222			[mitesh@Matrix find]$ find -perm -004
./file1							./file1
./file2							./file2
./file3							./file3
./file4							./file4
./file5							./file5
./file6							./file8

[mitesh@Matrix find]$ find -perm /111
./file3
./file4
./file5
{% endhighlight %}

#### Find and Numeric Criteria

* Many Find Criteria Take Numeric Value Such As
<pre>
/-----------------------------------------------------------------------\
|									|
|	-size:	|	The size of the file (k=KB, M=MB, G=GB)		|
|	-links:	|	Number of links to the file			|
|									|
|	-amin:	|	When the file was last read			|
|	-mmin:	|	When the file data last modified		|
|	-cmin:	|	when the file data/metadata last changed	|
|									|
|	-atime:	|	When the file was last read			|
|	-mtime:	|	When the file data last modified		|
|	-ctime: |	when the file data/metadata last changed	|
|									|
\-----------------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
* Files with a size of exactly 10MB
[mitesh@Matrix ~]$ find -size 10M

# Files with a size of over 10MB
[mitesh@Matrix ~]$ find -size +10M

# Files with a size of less than 10MB
[mitesh@Matrix ~]$ find -size -10M

# Looks for files on the system whose last accessed time stamp is exactly 5 days ago
[mitesh@Matrix ~]$ find / -atime 5

# Looks for files on the system whose last accessed time stamp is more than 5 days ago
[mitesh@Matrix ~]$ find / -atime +5

# Looks for files on the system whose last accessed time stamp is less than 5 days ago
[mitesh@Matrix ~]$ find / -atime -5

# List all the files in the /etc directory which are last accessed in less than 60 Minutes Ago
[mitesh@Matrix ~]$ find /etc -amin -60

# List all the files whose mtimes are newer than recentfile.txt
[mitesh@Matrix ~]$ find -newer recentfile.txt

# List all the files whose mtimes are older than recentfile.txt
[mitesh@Matrix ~]$ find -not -newer recentfile.txt
{% endhighlight %}

**NOTE!:** Use `stat` command to display files time stamps.
{: .notice}




* Find Can Also Execute Commands On The Found Files By Using The Following Options
<pre>
/-----------------------------------------------------------------------\
|									|
|	ok	|	Prompts Before Acting On Each Found Files	|
|	exec  	|	Runs The Commands Without Any Prompts		|
|									|
|	{}	|	File Name Placeholder				|
|	space\;	|	Terminate Command				|
|									|
\-----------------------------------------------------------------------/
</pre>

**For Example:**
{% highlight bash %}
# Fix other writable files in your home directory (Security Risk)
[mitesh@Matrix ~]$ find ~ -perm -002 -exec chmod o-w {} \;

# Fix any files that are writables by the others in the system (Security Risk)
[mitesh@Matrix ~]$ find / -perm -002 -exec chmod o-w {} \;

# Backup configuration files from the current directory and add .orig extension
[mitesh@Matrix ~]$ find -name "*.conf" -exec cp {} {}.orig \;

# Prompt to remove Joe's tmp files that are over 3 days old
[mitesh@Matrix ~]$ find /tmp -ctime +3 -user joe -ok rm {} \;

# Do an ls -l style listing of all directories in /home
[mitesh@Matrix ~]$ find /home -type d -ls

# Find all the .sh files in /data directory with a current permission of 644 and ask to make them executable
[mitesh@Matrix ~]$ find /data -type f -name "*.sh" -perm 644 -ok chmod 755 {} \;
{% endhighlight %}


### The Gnome Search Tool
* The Gnome search tool is accessed via Places -> Search for Files...
* The Gnome search tool only look at the user's home directory by default.
* The Gnome search tool uses the find command in the background, But not have all the find features.
