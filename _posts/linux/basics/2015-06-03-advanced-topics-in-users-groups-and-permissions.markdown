---
layout: post
title: "Advanced Topics in Users Groups and Permissions"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about manage users, groups and permissions"
tags: [Linux, Basics, Users, Group, Permissions]
image:
  feature:
date: 2015-06-03T13:06:44+05:30
---

{% include _toc.html %}


### Users & Group ID Numbers

* When Files Are Stored On The Computer, The Metadata About The File Is Stored Numerically.
* That Is, The Username And Group Affiliation Of The File Are Not Stored;
Rather, The User ID And Group ID Numbers Are Stored.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l
drwxr-xr-x. 2 mitesh mitesh 4096 Aug 26 12:11 Desktop

[mitesh@Matrix ~]$ ls -ln
drwxr-xr-x. 2 501 501 4096 Aug 26 12:11 Desktop
{% endhighlight %}


### Users & Group Informations Files

#### /etc/passswd
* The `/etc/passwd` file contains the list of the system's accounts.

**Format:**
{% highlight bash %}
Username:Password PlaceHolder:UID:GID:GECOS:Home Directory:Shell
{% endhighlight %}

**NOTE!:** GID Is The User's Primary Group ID Number
{: .notice}

#### /etc/shadow
* The `/etc/shadow` file contains the users encrypted passwords and account expiration information.
* The `/etc/shadow` file is not readable by anyone.

#### /etc/group
* The `/etc/group` file defines the groups on the system

**Format:**
{% highlight bash %}
Groupname:Password PlaceHolder:GID:UserList
{% endhighlight %}

**NOTE!:** UserList Is The List Of Usernames That Are Members Of This Group, Separated By The Commas.
{: .notice}

#### /etc/gshadow
* The `/etc/gshadow` file contains the groups encrypted passwords and the list of group administrators.
* The `/etc/gshadow` file is not readable by anyone.


### Users Management Tools

####CLI
  1. `useradd`:	Create a new user or update default new user information
  1. `usermod`:	Modify a user account
  1. `userdel[-r]`:	Delete a user account [Removes users Home Directory and users Mail Spool]

#### Graphical Tool

  1. system-config-users:
* System -> Administration -> Users and Groups
* Add Modify Deletes Users and Groups


### System Users and Groups
* In additions to the ordinary user accounts and the superuser root account,
The number of system users and groups exist.

* The main reason for creating system users and groups is,
Runs several programs as non-priviledged users or as a particular groups.
Examples: Daemon, mail, lp, nobody, web or print servers
* Running programs in this way limits the amount of damage any single program can do to the system.

* System Users & Groups All Have UID & GID Numbers Between The 1 & 499.

**Example:**
{% highlight bash %}
# Sample of /etc/passwd
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
apache:x:48:48:Apache:/var/www:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
rtkit:x:499:499:RealtimeKit:/proc:/sbin/nologin

# Sample of /etc/group
bin:x:1:root,bin,daemon
daemon:x:2:root,bin,daemon
mail:x:12:mail,postfix
ftp:x:50:
apache:x:48:
nobody:x:99:
rtkit:x:499:
{% endhighlight %}


### Monitoring Logins
  1. `w`:		Show who is logged on and what they are doing.

  1. `last`:	Show listing of last logged in users and reboot history. <br>
Examples: `last`; `last root`; `last mitesh`; `last reboot`;

  1. `lastb`:	Show bad login information.

  1. `lastlog`:	Reports the most recent login of all users or of a given user <br>
Examples: `lastlog`; `lastlog -u root`;



### Default Permissions
<pre>
Files:		0666 - umask
Directory:	0777 - umask
</pre>

#### umask

**Non-privileged user's umask is 0002**
<pre>
Files:      0666 - 0002 = 0664
Directory:  0777 - 0002 = 0775
</pre>

**Root user's umask is 0022**
<pre>
Files:      0666 - 0022 = 0644
Directory:  0777 - 0022 = 0755
</pre>

**Changing umask value**
{% highlight bash %}
[mitesh@Matrix ~]$ umask 022
[mitesh@Matrix ~]$ umask 0022
{% endhighlight %}

**NOTE!:**	The umask is typically set by the scripts run at the login time.<br>
That means your umask value is set to default everytime you login into the system.
{: .notice}



### Special Permissons For Executables
* In addition to the user, group and other permissions,
An additional set of permissions exist called special permissions.

  1. 4(s) The suid - set user id bit
  1. 2(s) The sgid - set group id bit
  1. 1(t) The sticky bit
<br><br>
* The special permission is displayed in the place of x.
  * Small Letter	= Executable Permission + Special Permission
  * Capital Letter	= No Executable Permission Only Special Permission


#### For Files

##### The SUID Permissions
* The command will run with the authority of the owner of the file,
Rather than, the authority of the user running the command.

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ passwd
{% endhighlight %}

**NOTE!:**	The `passwd` command changes a user's password,<br>
which is stored in the `/etc/shadow` file and it is not writable for non-privileged users.<br>
However, since the `passwd` command is owned by `root` and runs with the `suid` permissions,
Users running the command have the `root` privilege while changing their passwords.<br>
Hence, They have the permissions to edit the `/etc/shadow` file.
{: .notice}

{% highlight bash %}
[mitesh@Matrix ~]$ cp /usr/bin/whoami /tmp/
[mitesh@Matrix ~]$ chmod u+s /tmp/whoami
[mitesh@Matrix ~]$ /tmp/whoami
mitesh
[neo@Matrix ~]$ /tmp/whoami
mitesh

[mitesh@Matrix ~]$ chmod u-s /tmp/whoami
[mitesh@Matrix ~]$ /tmp/whoami
mitesh
[neo@Matrix ~]$ /tmp/whoami
neo
{% endhighlight %}

##### The SGID Permissions
* The command will run with the authority of the group of the file.



#### For Directory

##### The SGID Permissions
* The files created in this directory will inherit its group affiliation from the directory,
Rather than inheriting it from the user.
* The SGID Bit is commonly set for the Group Directories.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ chmod g+s GroupDir
[mitesh@Matrix ~]$ chmod 2775 GroupDir
[mitesh@Matrix ~]$ chmod g-s GroupDir
[mitesh@Matrix ~]$ chmod 775 GroupDir
{% endhighlight %}

#### The Sticky Bit
* The Sticky Bit For A Directory, Sets A Special Restriction On Deletion Of Files.
* If the sticky bit is set for the directory,
Then only the owner of the files or root can delete the files - Regardless of the write permissions of the directory.
* An Example of sticky bit set is `/tmp` directory

**Examples:**

{% highlight bash %}
[mitesh@Matrix ~]$ chmod o+t ~/tmp/
[mitesh@Matrix ~]$ chmod 1777 ~/tmp/
[mitesh@Matrix ~]$ chmod o-t ~/tmp/
[mitesh@Matrix ~]$ chmod 777 ~/tmp/

[mitesh@Matrix ~]$ ls -ld /tmp
drwxrwxrwt. 28 root root 4096 Jun  8 14:06 /tmp
{% endhighlight %}
