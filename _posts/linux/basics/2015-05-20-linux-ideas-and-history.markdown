---
layout: post
title: "Linux Ideas and History"
comments: true
modified:
categories: linux/basics
excerpt: "History behind Linux and OpenSource"
tags: [Linux, Basics, Tutorials, History]
image:
  feature:
date: 2015-05-20T15:04:00+05:30
---
{% include _toc.html %}


## Open Source


#### The Open Source Initiative

* The software and source code freely available to all.
* The software and source code freely distributable.
* The ability to modify the source code and create derived works.
* To maintain the integrity of original author's work, the license may require
  that changes to the code be provided in patch form.
* The license must be inherited, so those who receive the distribution are subject to the same terms.
* The license must be nondiscriminatory with respect to person, groups, or field of endeavor; it must be free of restriction that can limit the license.

#### The Free Software Foundation

* The software must be freely executable for any purpose.
* The source code must be available so that others can study how it works.
* The software must be freely redistributable.
* All are free to modify the software.


## Linux Origins

In 1984 FSF started a GNU Project, which was created to develop a UNIX like operating system.
The GNU Project began by writing replacement tools for UNIX utilities (such as bash, ls etc), eventually creating a complete set of tools, libraries, and other materials. To advance their ideas of software freedom, they created and then put their software under the General Public License (GPL), a license that enforces the Four Freedoms espoused by the FSF.
Today, most of the utilities and applications included with Red Hat Linux are also covered by the GPL.

In 1991 Linus Benedict Torvalds, a graduate student in Helsinki University Finland, creates Open Source UNIX like kernel, released under the GPL.

Today,
	Linux kernel + GNU utilities = complete open source UNIX like operating system

A kernel is the most fundamental part of an operating system, providing services to other user-level commands, such as the ability to communicate with hard disk and other pieces of hardware.



## Linux Principles


#### 1. Everything is a file - including hardware

* Linux and UNIX system has many powerful utilities designed to create and manipulate files.
* The Linux and UNIX security model is based around the security of files.
* By treating everything as a file, a consistency emerges.
* You can secure access to hardware in the same way as you can secure access to a document.

#### 2. Small single-purpose programs

* Linux and UNIX provides many small utilities that perform one task very well.
* When new functionality is required, the general philosophy is to create a separate program - rather than to extend an existing utility with new features.

#### 3. Ability to chain programs together to perform complex tasks

* A core design feature of Linux and UNIX is that the output of one program can be the input for another program.
* This gives the user flexibility to combine many small programs togather to perform  a larger, more complex tasks.

#### 4. Avoid captive user interface

* Interactive command are rare in Linux and UNIX.
* Most commands expect their options and arguments to be typed on the command line when the command is launched.
* The command completes normally, possibly producing output, or generates an error message and quits. * Interactivity is reserved for programs where it makes sense, for example, text editors (of course, there are non-interactive text editors too.)

#### 5. Configuration data stored in text:

* Text is a universal interface, and many Linux and UNIX utilities exist to manipulate text.
* Storing configuration in text allows an administrator to move a configuration from one machine to another machine easily.
* There are several revision control applications that enable an administrator to track which change was made on a particular day, and provide the ability to roll back a system configuration to a particular date and time.
