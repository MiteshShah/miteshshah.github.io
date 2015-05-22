---
layout: post
title: "Browsing the Filesystem"
comments: true
modified:
categories: Linux, Basics, CLI
excerpt: "Newbie Guide - Basic Linux Commands & Filesystem Details"
tags: [Linux, Basics, Tutorials]
image:
  feature:
date: 2015-05-21T20:57:59+05:30
---
{% include _toc.html %}

### Linux File Hierarchy Concepts

* Files and Directories are organized into a single rooted inverted tree structure, including distinct physical volumes such as Floppy Disks, CD-ROMs and Multiple Hard Drives.
* Filesystem begins at the root directory, represented by lonely forward slash (/).
* Names in Linux File Hierarchy are case sensitive.
* Paths are delimited by / such as `/usr/share/bin/X11/X`
* Each shell and system process has a current working directory.

*	`.`  refers to the current working directory.
*	`..` refers to the parent directory of any particular directory - just one level up in the file hierarchy.

* Files and Directories whose name begin with a `.` are hidden.
* A user's path is a list of directories that are searched for commands typed at the command line.


### Some Important Directories

1. Home Directories:	`/root`,	`/home/username`
1. User Executables:	`/bin`,	`/usr/bin`,	`/usr/local/bin`
1. System Executables:	`/sbin`,	`/usr/sbin`,	`/usr/local/sbin`
1. Shared Libraries: `/lib`,	`/usr/lib`,	`/usr/local/lib`
1. Kernels &  Boot-loaders:	`/boot`
1. Configuration Files:	`/etc`
1. Device Files:		`/dev`
1. Temporary Files:	`/tmp`
1. Other Mount-points:	`/media`,	`/mnt`
1. Server Data:		`/var`,	`/srv`
1. System Informations:	`/proc`,	`/sys`
1. Optional Applications:	`/opt`


#### 1. Home Directories
* Every user has a home directory.
* The root user's home directory is `/root`.
* Most non-root user's home directories are in `/home/username`.

#### 2/3. User/System Executables

* The essential binaries reside in `/bin` for user and `/sbin` for systems.
* The non essential binaries, such as graphical environments or office tools installed in `/usr/bin` and `/usr/sbin`.
* The software compiled from the source code, usually go in `/usr/local/bin` and `/usr/local/sbin`.

#### 4. Shared Libraries
* The `lib` directory contains the libraries that provide shared code used by many Linux Applications.

#### 5. Kernels & Boot-loaders
* The boot loader is in charge of loading the core of the Linux, called kernel, into  the memory.
* The boot loader, kernel and loader's configuration files are stored in `/boot`.

#### 6. Configuration Files
* Most of the configuration files are stored in `/etc` and its subdirectories.

#### 7. Device Files
* Most of the device files are stored in `/dev` and its subdirectories.

#### 8. Temporary Files
* The `/tmp` directory is usually used by many Linux Applications for storing temporary data.
* Once a day system automatically deletes any files over ten days old in `/tmp`.

#### 9. Other Mount-points
* Filesystems of the removable media are usually mounted under `/media`.
* For Example: A cdrom is usually mounted under `/media/cdrom`.


#### 10. Server Data
* The `/var` directory contains the regularly changing system files such as logs, print spools and email spools.
* The `/srv` directory contains the server data such as databases and web pages.

#### 11. System Informations
* The `/proc` directory is a special dynamic directory that provides the informations about a running Linux system and allows some tweaking while a system is running.
* The `/sys` directory is related to the hardware.

#### 12. Optional Applications
* The `/opt` directory provides a location for optional applications to be installed.


### Files and Directory Names

* Names may be up to 255 characters.
* Names are case  sensitive.
  * For Examples: MAIL, Mail, mail and maiL
  * Again, possible, but not be wise.
* All characters are valid, except forward-slash (/).
  * It maybe unwise to use certain special characters in files and directory names.
  *	Among the characters to avoid are: <>?*" and quotation marks, as well as spaces, tabs, and other non-printable characters.
  * Some characters should be protected with quotes when referencing them.
{% highlight bash %}
ls -l "file name with spaces.txt"
-rw-rw-r--. 1 mitesh mitesh 0 Aug 12 18:44 file name with spaces.txt
{% endhighlight %}

**NOTE!:** Absent the quotes, you would be asking the system to list the four different files.
{: .notice}

### Absolute and Relative Pathnames

#### Absolute Pathnames
* Begin with a forward slash (/).
* Complete road map to the file location.
  * For Example: `/usr/share/doc/HTML/index.html`
* Can be used anytime you wish to specify a file name, Regardless of the current working directory.

#### Relative Pathnames
* Do not begin with a forward slash.
* Specify location relative to your current working directory.
* Can be used as a shorter way to specify the file name.


### Current Working Directory

* Each shell and system process has a current working directory.
* The `pwd` command displays the absolute path of the shell's current working directory.
{% highlight bash %}
[mitesh@Matrix ~]$ pwd
/home/mitesh
{% endhighlight %}

### Changing Directories - cd command

* The `cd` command are used to change the directory.
* The only argument to the `cd` command is either an absolute or relative pathname, or a shortcut representing the directory to which you wish to change.

**To absolute or relative path**
{% highlight bash %}
cd /home/mitesh/Desktop
cd projects/docs
{% endhighlight %}

**To your home directory**
{% highlight bash %}
cd
cd ~
{% endhighlight %}

**To a directory just one level up**
{% highlight bash %}
cd ..
{% endhighlight %}

**To your previous working directory**
{% highlight bash %}
cd -
{% endhighlight %}


### Listing Directory Contents - ls command

* List the contents of the current working directory or a specified directory.

{% highlight bash %}
ls [OPTION]... [FILE]...
{% endhighlight %}

{% highlight bash %}
ls;     ls -l;      ls -R
ls -a;  ls -l /usr;
ls /;   ls -ld /usr;
{% endhighlight %}

**List the files and directories in the current working directory**
{% highlight bash %}
[mitesh@Matrix ~]$ ls
work
{% endhighlight %}

**List the hidden files and directories**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -a
.bash_history  .bash_logout  .bash_profile  work
{% endhighlight %}

**List the files and directories of the specified directory**
{% highlight bash %}
[mitesh@Matrix ~]$ ls /
bin   cgroup  etc   lib    lost+found  misc  net  proc  sbin     srv  tmp  var
boot  dev     home  lib64  media       mnt   opt  root  selinux  sys  usr
{% endhighlight %}

**Long Listing**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l /usr
total 148
dr-xr-xr-x.   2 root root 36864 Aug  8 21:51 bin
drwxr-xr-x.   2 root root  4096 Dec  4  2009 etc
drwxr-xr-x.   2 root root  4096 Dec  4  2009 games
...output truncated...
{% endhighlight %}

**Display directory information, not their contents**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -ld /usr
drwxr-xr-x. 13 root root 4096 Aug  4 20:58 /usr
{% endhighlight %}

**NOTES!:** It has no effect when filenames are passed as arguments.
{: .notice}
<br>
<br>**Recurse though directories**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -R
{% endhighlight %}


### Copying Files and Directories - cp command

* The cp command is used to copy files and directories.
{% highlight bash %}
cp [OPTION]... SOURCE DEST
cp [OPTION]... SOURCE... DIRECTORY
{% endhighlight %}


**Options**
<pre>
* -i(interactive):	Ask before overwriting a file
* -r(recursive):	Recursively copy an entire directory tree
* -p(preserve):         Preserve the permissions, ownership, and time stamps
* -a(archive):	        Copy files and directories recursively (like -r) while preserving permissions (like -p)
</pre>


**The Destination**

* If the destination is a directory, a copy of the source file is placed in that directory with the same name as the source.
* If the destination is a file, a copy of the source file is created with that destination name.

<a href="#the-root-user" class="btn btn-danger">WARNING</a>
THE CP COMMAND ALWAYS OVERWRITES THE DESTINATIONS!!
{: .notice}

{% highlight bash %}
[mitesh@Matrix ~]$ ls /home/mitesh/
testfile

[mitesh@Matrix ~]$ cp ~mitesh/testfile /tmp/
[mitesh@Matrix ~]$ ls /tmp/
testfile
... other output omitted...

[mitesh@Matrix ~]$ cp ~mitesh/testfile /tmp/mitesh_test_file
[mitesh@Matrix ~]$ ls /tmp/
mitesh_test_file
... other output omitted...
{% endhighlight %}

### Moving and Renaming Files and Directories - mv command

* The mv command is used to move/rename files and directories.
{% highlight bash %}
mv [OPTION]... SOURCE DEST
mv [OPTION]... SOURCE... DIRECTORY
{% endhighlight %}

* Aside from a couple of switches, mv and cp function identically -\- The only difference is that with cp source and destination both are presents and with mv source disappears only destination is there.

* Destination works like cp command.

{% highlight bash %}
[mitesh@Matrix ~]$ ls /home/mitesh/
testfile

[mitesh@Matrix ~]$ mv ~mitesh/testfile /tmp/
[mitesh@Matrix ~]$ ls ~mitesh
[mitesh@Matrix ~]$ ls /tmp/
testfile
... other output omitted...

[mitesh@Matrix ~]$ mv ~mitesh/testfile /tmp/mitesh_test_file
[mitesh@Matrix ~]$ ls ~mitesh
[mitesh@Matrix ~]$ ls /tmp/
mitesh_test_file
... other output omitted...
{% endhighlight %}



### Creating and Removing Files and Directories

#### Touch command
* The touch command is used to creates empty files or update file timestamps
{% highlight bash %}
touch [OPTION]... FILE...
{% endhighlight %}


#### rm command
* The rm command is used to remove files.
{% highlight bash %}
rm [OPTION]... FILE...
{% endhighlight %}

**Options**

* -i(interactive):	Prompt before every removal
* -f(forcefully):		Forcefully removed without any prompt
* -r(recursive):		Remove directories and their contents recursively

{% highlight bash %}
[mitesh@Matrix notes]$ ls
file1  file2  file3  file4  file5

[mitesh@Matrix notes]$ rm -i file1
rm: remove regular file `file1`? y
[mitesh@Matrix notes]$ ls
file2  file3  file4  file5

[mitesh@Matrix notes]$ rm -f file2
[mitesh@Matrix notes]$ ls
file3  file4  file5

[mitesh@Matrix ~]$ rm -ri notes/
rm: descend into directory `notes`? y
rm: remove regular empty file `notes/file1`? y
rm: remove regular empty file `notes/file2`? y
rm: remove directory `notes'? y

[mitesh@Matrix ~]$ rm -rf notes/
{% endhighlight %}

#### mkdir command

* The mkdir command is used to creates the directories.
{% highlight bash %}
mkdir [OPTION]... DIRECTORY...
{% endhighlight %}

**Options**

* -p(parents):	Make parent directories as needed

{% highlight bash %}
[mitesh@Matrix ~]$ mkdir work
[mitesh@Matrix ~]$ ls
work

[mitesh@Matrix ~]$ mkdir -p work/labs
[mitesh@Matrix ~]$ ls
work
[mitesh@Matrix ~]$ ls work/
labs
{% endhighlight %}

#### rmdir command
* The rmdir command is used to remove empty directories.
{% highlight bash %}
rmdir [OPTION]... DIRECTORY...
{% endhighlight %}

**NOTE!:** The `rmdir` command removes only empty directories. <br>
To remove a directories and its contents, use `rm -r`.
{: .notice}


### Using Nautilus
* Nautilus is a GNOME Graphical File System Browser.
* Nautilus can run in one of two modes:

#### 1. Spatial Mode

* Spatial mode is designed for new users and the simplest in the terms of user interface clutter.
* Windows have a very basic layout with no toolbar and when a directory is double-clicked it opens in its own window.
* A menu in the lower-left of each window allows the user to list and select parent directories.

#### 2. Browser Mode

* Browser Mode provides a more traditional file manager interface.
* Windows have a very advance layout with toolbar, menubar, and side panel and when a directory is double-clicked it opens in the same windo.
* Browser mode can be started by selecting Application -> System Tools -> File Browser

**Nautilus can be accessed in a number of ways**

* Desktop Icons
	* Home:		Your home directory
	* Computer:	Root filesystems, Network resources, and Removable media

**Nautilus Shortcuts**

* `Ctrl+c`	Copy
* `Ctrl+v`	Paste
* `Ctrl+a`	Select All
* `Ctrl+l`	Open Location dialog
* `Ctrl+q`	Close all nautilus windows
* `Ctrl+Shift+w`	Close all the parent windows
* `Ctrl+Shift+n`	Creates new Directory

**Moving and Copying in Nautilus**

  1. Drag-and-Drop
  * `Drag`:		Move on same filesystem, copy on different filesystem
  * `Drag+Ctrl`:	Always Copy
  * `Drag+Alt`:	Ask whether to copy,move or creates symbolic link (alias)

  2. Context Menu
  * Right-click to rename, cut, copy or paste


### Determining File Content

* Check file type before opening it with file command.
* `file` command prints its best guess of the type of data contained in a file. (by using `/usr/share/magic`)
* Files can contains many types of data such as ASCII (Plain Text, HTML, Executable Shell Scripts, C Program Source Code, Mailbox-format Text) or Binary ( Compiled Executables, Compressed Data, Images and Sound Samples).
