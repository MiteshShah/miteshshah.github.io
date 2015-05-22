---
layout: post
title: "Linux Usage Basics"
comments: true
modified:
categories: linux/basics
excerpt: "Newbie Guide - How to use Linux"
tags: [Linux, Basics, Tutorials]
image:
  feature:
date: 2015-05-20T17:50:19+05:30
---
{% include _toc.html %}

### Logging in to a Linux System

* Access to the system requires authentication.
* The most common method used to authenticate a user is a login process, by entering a valid username and password.

#### Two types of login screens
1. Virtual Consoles (Text Based)
1. Graphical Logins (Display Manager)

##### 1. Virtual Consoles (Text Based)

**Note!** Your password is not displayed when typed. After login in to a linux system you will have a command prompt, probably ending in a dollar sign ($).
{: .notice}

{% highlight bash %}
# For Example:
Matrix login: mitesh
Password:
Last login: Wed May 20 18:00:00 on tty1
[mitesh@Matrix ~]$
{% endhighlight %}

##### 2. Graphical Logins (Display Managar)

On systems that boot directly into the X Window System, What you see next depends upon the default display manager being used. The default display manager for most of Linux is GDM (GNOME Display Manager) and again by default, the GDM start the GNOME Desktop.


### Switching between Virtual Consoles and Graphical Environment

* A typical Linux systems will run six virtual consoles and one graphical environment.
* Server systems often have only virtual consoles.
* Desktops and workstations typically have both.
* Switching among virtual consoles by typing:	Ctrl+Alt+F1 through Ctrl+Alt+F6
* Access to the graphical environment by typing:	Ctrl+Alt+F7

**Note!** Virtual consoles keep a history of data displayed on the screen. Users can scroll back to data already off the screen by typing Shift+PgUp and Shift+PgDn. Be aware that the scroll buffer is stored in video memory, and so the scroll buffer will be lost when you change to another virtual console.
{: .notice}


### Elements of the X Window System

* The X window System is Linux's Graphical Subsystem.
* X is a client/server protocol that regulate the communication between the applications (clients) and the system that provides the display services (server); X does not define the actual looks and behavior of the windowing system: it does not define the buttons, menus, toolbars, etc. Rather, it defines the communication between the applications (clients) and the system that provides the display services (server).

**Note!** Red Hat's provider of X is Xorg, an open source group that supplies X related packages, including the server and many clients.
{: .notice}

* The actual looks and behavior largely controlled by the Desktop Environment.
* Most Common Desktop Environments

  1. GNOME (GNU Network Object Model Environment)
  2. KDE	 (K Desktop Environment)

Both provides consistent user interface, panels for managing menus and application launching, and sets of standard X-based tools.


#### Starting the X Server

Depending on the configuration of the particular system, The X server starts automatically at boot time; Otherwise if systems come up in virtual consoles, users must start the X server manually.

* The X server must be pre-configured by the system administrator.
* Login into virtual console and run startx command.
* The system will start the X server on Ctrl+Alt+F7 and automatically switch to the X server.


### Changing Your Password

The combination of login name and password control access to the system.

##### General guidelines for best security
* Change the password the first time you log in.
* Change it regularly thereafter.
* Select the password that is hard to guess.

##### Rules for creating a strong password

* Use minimum eight characters; more characters are better, as long as you are comfortable to remember them and typing them.
* Do not base the password on a dictionary word.
* Use a variety of different types of characters; use at least three of the following:
* Lower case letters
* Upper case letters
* Numbers
* Punctuation
* Special symbols
* Avoid using your real name, login name, or variations thereof. For example: with a login name of sally, a poor password would be s@lly.
* Avoid using easy to determine personal information, such as your birthday, anniversary, etc.
* Avoid using formulas, such as 1+1=2.
* Avoid excessive complexity if it tempts you to perform such unsafe practices as writing the password onto a notepad near your monitor.


**Note!** To change your password from terminal, use passwd command.
Before the password can be changed, the current password must be supplied. Note that the characters you enter when typing a password are never echoed back to the screen.
By default, the system performs some checks to ensure that a week password is not chosen. If you enter such a password, the system will return an error message and allow you to try again.
{: .notice}

### The root user

* The root user is a special administrative account on Linux system, also called the superuser account.
* The root user has near complete control over the system, and a nearly unlimited capacity to damage it!

<a href="#the-root-user" class="btn btn-danger">WARNING</a>
Do not login as root user unless it is absolutely necessary.<br>
And then logout of the root user as soon as possible.
Normal (unprivileged) users' protential to do damage is more limited.
{: .notice}

### Changing Identities

##### 1. su command

* The su command is used to change the identities.
* By default, su assumes that you wish to become a root.
* If a username is passed (su - mitesh), the resulting shell will run as that user instead.
* The su command always prompts for the user's password unless you are running it as root.
* The root user may access any account without providing a password.

* When a - is passed as an argument to su ( su - ), a login shell is created. Otherwise, a non-login shell is created.

**NOTE!:** Especially when becoming root user, certain important settings are inherited by login shells, but not by non-login shells. Hence it is considered best practice to always use the - option when running the su command.
{: .notice}

##### 2. sudo command

* The sudo command is used to elevate your privileges just for runtime of the command.
* For example: `sudo passwd joe` would run the passwd command as root user, allowing a non-root user to change joe's password.

**NOTE!:** In order to use sudo command, a system administrator must have granted you access ahead of time and can control what commands you may run with elevated privileges.
{: .notice}

**NOTE!:** Usually, sudo will prompt you for a password. The password being requested is your password, not root's.
{: .notice}

##### 3. id command:

* The id command is used to check who you are and what groups you are in.
* id -Z prints only the security context of the current user.
* You can also view the information of other users by running id username.


### Editing Text Files

* vi:	Advance, full feature text editor.
* vim:	Upgraded version of vi.
* gvim:	Graphical version of vim.
* gedit:	Simple graphical text editor.
* nano:	Easy to learn, easy to use.
