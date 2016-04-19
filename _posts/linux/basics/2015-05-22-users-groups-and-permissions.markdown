---
layout: post
title: "Users Groups and Permissions"
comments: true
modified:
categories: linux/basics
excerpt: "Newbie Guide - Linux users, groups and Permissions"
tags: [Linux, Linux Basics, User, Group, Permissions]
image:
  feature:
date: 2015-05-22T11:59:52+05:30
---

{% include _toc.html %}

### Users
* Every user is assigned a unique User ID Number (UID).
* UID 0 identify as root
* User accounts normally start at UID 500 (Redhat) or 1000 (Debian)
* User's Names and UIDs are stored in `/etc/passwd` file.
* Users are assigned a home directory and a program that is run when users are log in (usually a shell).
* Users cannot read, write or execute each other's files without permissions.


### Groups
* Users are assigned to groups.
* Every group is assigned a unique Group ID Number (GID).
* Group's Names and GIDs are stored in `/etc/group` file.
* Each users is given their own private group.
* By default the groupname is same as their usernames.
* For example user mitesh is a member of group mitesh and by default, is the only member of that group.
* Users can be added to other groups for additional access.
* All users in a group can share files that belong to the group.


#### Primary and Secondary Group
* A user's primary group is defined in the `/etc/passswd` file.
* A user's secondary groups are defined in the `/etc/group` file.
* The primary group is important because files created by this user will inherit that group affiliation.
* The primary group can temporarily changed by running
{% highlight bash %}
newgrp groupname
{% endhighlight %}

where groupname is one of the user's secondary groups. The user can return to their original group by typing `exit`.


### Linux File Security
* Every file is owned by a UID and a GID.
* Every process runs under the authority of a particular user(UID) and with the authority of one or more groups(GIDs). This is called process security context.

#### Three Access Categories
* Process running with the same UID as the File (User)
* Process running with the same GID as the File (Group)
* All other Processes (Other)


#### Permission Precedence
* Process running with the same UID as the File, User permissions apply.
* Process running with the same GID as the File, Group permissions apply.
* Process running with the different UID and GID then Other permissions apply.


### Permission Types

#### File Permissions
{% highlight bash %}
-   No Permission.
r   Permission To Read The File Content.
w   Permission To Write To The File.
x   Permission To Execute A Program.
{% endhighlight %}

#### Directory Permissions
{% highlight bash %}
-   No Permission.
r   List The Directory Contents.
w   Creates Or Remove Files From A Directory.
x   Change Into A Directory And Do A Long Listing.
{% endhighlight %}

<a href="#directory-permissions" class="btn btn-danger">WARNING</a>
A file may be removed by anyone who has a write permission to the directory in which the file resides, regardless of the ownership or permissions on the file.
{: .notice}

#### Examining Permissions

* File and Directory permissions may be viewed by `ls -l` command.

{% highlight bash %}
[mitesh@Matrix ~]$ ls -l /bin/login
-rwxr-xr-x. 1 root root 30992 Aug 13  2010 /bin/login
{% endhighlight %}

#### Interpreting Permissions
{% highlight bash %}
-rwxr-x--- 1 andersen trusted 2948 Oct 11 14:07 myscript
{% endhighlight %}

* Read, Write and Execute for the owner, andersen
* Read and Execute for members of the trusted group
* No access for all others

{% highlight bash %}
-rwxr-xr-- 1 fred fred	26807	Mar 8 22:55  penguin
-rw-r--r-- 1 mary admin	 1601	Mar 5 22:36  redhat
-rw-rw-r-- 1 root staff	 8671	Mar 8 22:59  tuxedo
{% endhighlight %}
<pre>
/---------------------------------------------------------------\
|	User	|	Primary Group  |	Secondary Group |
|---------------------------------------------------------------|
|	fred	|	fred           |	staff           |
|	mary	|	mary           |	staff,admin     |
\---------------------------------------------------------------/
</pre>

* The penguin can be read, wrire and executed by fred, but only read by mary.
* The redhat can be read and write by mary, but only read by fred.
* The tuxedo can be read and write by both mary and fred.



### Changing File Ownership

#### Ownership

* Only root can change the file's owner.
* Ownership is changed with `chown` command:
{% highlight bash %}
chown [OPTION]... [OWNER][:[GROUP]] FILE...
{% endhighlight %}

**For Example:**

* The following command would grant ownership of the file foofile to mitesh
{% highlight bash %}
chown mitesh foofile
{% endhighlight %}
* The following command would grant ownership of foodir and all the files and subdirectories within it to mitesh:
{% highlight bash %}
chown -R mitesh foodir
{% endhighlight %}

#### Group-Ownership

* Only root or the owner can change the file's group.

**NOTE!:** root can grant ownership to any group, while non-root users can grant ownership only to groups they belong to.
{: .notice}

* Group-Ownership is changed with `chgrp` command
{% highlight bash %}
chgrp [OPTION]... GROUP FILE...
{% endhighlight %}

**For Example:**

* The following command would grant group-ownership of the file foofile to mitesh
{% highlight bash %}
chgrp mitesh foofile
{% endhighlight %}
* The following command would grant group-ownership of foodir and all the files and subdirectories within it to mitesh
{% highlight bash %}
chgrp -R mitesh foodir
{% endhighlight %}


### Changing Permissions
* To change access modes
{% highlight bash %}
chmod [OPTION]... MODE[,MODE]... FILE...
{% endhighlight %}

#### Symbolic Method

* Where Mode is
<pre>
/------------------------------------------------------------\
|	Who       |	Operator  |	Permission	     |
|------------------------------------------------------------|
|	u User    |	+ Add     |	r Read		     |
|	g Group   |	- Remove  |	w Write		     |
|	o Other   |	= Assign  |	x Execute	     |
|	a All     |               |	s SUID		     |
|                 |               |	t Sticky Bit         |
\------------------------------------------------------------/
</pre>

**Examples**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l .bashrc
-rw-r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod u+x,go-r .bashrc
-rwx------. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod ug+rw .bashrc
-rwxrw----. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod ug=rw,o+x .bashrc
-rw-rw---x. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod -rw .bashrc
---------x. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod +r .bashrc
-r--r--r-x. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod -x .bashrc
-r--r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod u+w .bashrc
-rw-r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc
{% endhighlight %}


#### Numeric Method

* Uses a Three Digit Mode Number
<pre>
/---------------------------------------------------------------\
|		who			|	Permission	|
|---------------------------------------------------------------|
| 	1st Digit: Owner Permission	|	4  Read		|
|	2nd Digit: Group Permission	|	2  Write	|
|	3rd Digit: Other Permission	|	1  Execute	|
\---------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l .bashrc
-rw-r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod 664 .bashrc
-rw-rw-r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod 660 .bashrc
-rw-rw----. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod 444 .bashrc
-r--r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc

[mitesh@Matrix ~]$ chmod 644 .bashrc
-rw-r--r--. 1 mitesh mitesh 124 Jun 22  2010 .bashrc


[mitesh@Matrix ~]$ ls -ld dir
drwxrwxr-x. 2 mitesh mitesh 4096 Aug 20 16:30 dir

[mitesh@Matrix ~]$ chmod 755 dir
drwxr-xr-x. 2 mitesh mitesh 4096 Aug 20 16:30 dir

[mitesh@Matrix ~]$ chmod 770 dir
drwxrwx---. 2 mitesh mitesh 4096 Aug 20 16:30 dir

[mitesh@Matrix ~]$ chmod 700 dir
drwx------. 2 mitesh mitesh 4096 Aug 20 16:30 dir

[mitesh@Matrix ~]$ chmod 555 dir
dr-xr-xr-x. 2 mitesh mitesh 4096 Aug 20 16:30 dir

[mitesh@Matrix ~]$ chmod 775 dir
drwxrwxr-x. 2 mitesh mitesh 4096 Aug 20 16:30 dir
{% endhighlight %}

#### Nautilus

* In a Nautilus window, right-click on a file.
* Select Properties from the context menu.
* Select the Permission tab.
