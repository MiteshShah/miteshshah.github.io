---
layout: post
title: "System Initialization"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about Linux system boot sequence"
tags: [Linux, Boot, Kernel]
image:
  feature:
date: 2015-06-10T12:29:33+05:30
---

{% include _toc.html %}

### System Initialization

#### Checking Your System State

{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/redhat-release
Red Hat Enterprise Linux Server release 6.0 (Santiago)

# Identifying Kernel
[mitesh@Matrix ~]$ uname -r
2.6.32-71.el6.x86_64

[mitesh@Matrix ~]$ yum list installed kernel\*
[mitesh@Matrix ~]$ rpm -qa kernel\*
kernel-firmware-2.6.32-71.el6.noarch
kernel-2.6.32-71.el6.x86_64
{% endhighlight %}


#### Runlevels
* The Runlevel Determines Which Services Are Started Automatically On Your Linux System.
* The Runlevel Is Selected By

  * Passing An Argument From The Bootloader
  * The Default Runlevel Is Stored In /etc/inittab File
    `id:5:initdefault:`
  * Using The Command Line: init New_Runlevel

##### Linux Default Runlevels
<pre>
/-----------------------------------------------------------------------------------------------\
|												|
|	0		|	Halt - Shutdown To A Poweroff State				|
|			|	shutdown -h now							|
|												|
|	1,S		|	Single User Mode						|
|	s,single	|	Run rc.sysinit							|
|												|
|	emergency	|	Bypass rc.sysinit, sulogin - prompts for root password		|
|												|
|	2		|	Multiuser Text Mode Without NFS					|
|												|
|	3		|	Full Multiuser Text Mode With NFS				|
|												|
|	4		|	Unused								|
|												|
|	5		|	X11								|
|												|
|	6		|	Reboot - Shutdown & Soft Reboot					|
|			|	shutdown -r now							|
|												|
\-----------------------------------------------------------------------------------------------/
</pre>
{% highlight bash %}
# Identifying Runlevel
[mitesh@Matrix ~]$ who -r
[mitesh@Matrix ~]$ /sbin/runlevel

# Default Runlevel
[mitesh@Matrix ~]$ grep initdefault: /etc/inittab
{% endhighlight %}

#### Controlling Services

* The Job Of The System V Initialization Scripts Is To Start Services At Boot Time.
* Most Of These Services Are Run As Daemons Such As `cups`, `crond` and `sendmail`.

##### The Service Command

* The service command is used to `status` `start` `stop` `restart` A Standalone Service Immediately

  * `service --status-all`
  * `service servicename status | start | stop | restart | reload | condrestart`

**NOTE!:** The Effect Of service command Is Temporarily
The Effect Of service command Is Lost After The Reboot.
{: .notice}

##### The ntsysv Command

* The `ntsysv` command Is Used To Configuring Runlevel Services
* By Default The `ntsysv` command Configures The Current Runlevel
* By Using `ntsysv --level` Option You Can Configure Other Runlevels

**NOTE!:**	After Reboot Is Take Effect & Permanent For Specific Runlevel
{: .notice}

##### The chkconfig Command

* `chkconfig --list` Display The List Of Services
* Whether They Are Started | Stopped In Each Runlevels
{% highlight bash %}
# Start | Stop Service In Runlevel 2,3,4,5
[mitesh@Matrix ~]$ chkconfig servicename on | off

# Start | Stop Service In Runlevel 2 And 4
[mitesh@Matrix ~]$ chkconfig --level 24 servicename on | off
{% endhighlight %}

**NOTE!:** After Reboot Is Take Effect & Permanent
{: .notice}

##### The system-config-services

* `System -> Administration -> Services`
* The `system-config-services` command Opens A X Client in GUI
* The system-config-services command Display The List Of Services

* By Using GUI Of The `system-config-services `command
* You Can Start | Stop | Restart The Services In Particular Or All The Runlevels


### Boot Sequence

1. BIOS Initialization
1. GRUB Bootloader
1. Kernel Initialization
1. Init Initialization

#### 1. BIOS Initialization

<pre>
| Reset SMPS |--------CMOS-------| CPU |------------------| BIOS |
</pre>

* POST - Power On Self Test
* Initialize All The Hardwares
* Checks For The Bootable Devices
FDD, HDD, USB, CD-ROM, DVD
* Check For The Boot Sector MBR - Master Boot Records
<pre>
/-----------------------------------------------------------------------\
|				512 Bytes				|
|----------------------------------------------------------------------
|									|
|	  446 Bytes	|	64 Bytes	|	2 Bytes		|
|			|			|			|
|	   NTLDR	|	Partition	|	0xAA55		|
|	    IPL		|	Table		|			|
|	(GRUB Stage 1)	|	Information	|	OS Signatures	|
|									|
\-----------------------------------------------------------------------/
</pre>
**NOTE!:** NTLDR = Windows Boot Loader<br>
IPL   = Initial Program Loader
{: .notice}

##### Partition Table Information
<pre>
/-----------------------------------------------------------------------\
|				64 Bytes				|
-----------------------------------------------------------------------
|	16 Bytes	|	1	|	Primary Partition	|
|			|---------------|				|
|	16 Bytes	|	2 	|	Primary Partition	|
|			|---------------|				|
|	16 Bytes	|	3	|	Primary Partition	|
|			|---------------|				|
|	16 Bytes	|	|-------|				|
|			|   4   |-------|	Extended Partition	|
|			|	|-------|				|
\-----------------------------------------------------------------------/
</pre>


#### 2. GRand Unified Bootloader - GRUB

* The Bootloader Is Responsible For Loading And Starting The Linux OS.

##### GRUB Stages

**1st Stage:**
* Small, Added To MBR Or Boot Sector During The Installation
* Loads The Grub 2nd Stage Into Memory
* Repair GRUB:
{% highlight bash %}
# Method 1
[root@Matrix ~]# /sbin/grub-install /dev/sda

# Method 2
[root@Matrix ~]# grub
grub> root (hd0,0)
grub> setup (hd0)
grub> quit
{% endhighlight %}

**2nd Stage:**

* Loaded From Filesystem Containing `/boot`
* Configuration: `/boot/grub/grub.conf`


##### The GRUB Boot Screen

* When Grub Starts Press The Esc Key
* Then Grub Splash Screen Has Been Display
With The List Of Menu Entries (Boot Images)

* Select The Menu Entry With The Help Of Up & Down Arrow Keys
* After That Press Enter For Boot The System

* If Grub Password Is Set Then Press The Password(p) Key
* Then Provide Your Grub Password To Unlock The Grub Menu

* You Can Modify The Grub Menu Entry With The Help Of Edit(e) Key
* After Modify The Menu Entry Press Boot(b) Key For Boot The System

* To Access The Grub Command Line Then Press The Command(c) Key


##### Argument Passing

* Change An Existing Boot Stanza In The Menu Editing Mode
Such As Change The Root Filesystem, Kernel And Runlevel

* In Grub CLI
  * Experiments With GRUB
  * Perform Diagnostic Tests
  * View The Contents Of The File


##### Password Protection

* Block Menu Editing Mode
* Block Particular Menu Entry (Boot Image) Selection
{% highlight bash %}
# Generate Grub Password:
[root@Matrix ~]# grub-md5-crypt
Password:
Retype password:
$1$3SoEu8O7$AmXQgjKBvRKPL5jxC5oBj1

[root@Matrix ~]# vim /boot/grub/grub.conf
password --md5 $1$3SoEu8O7$AmXQgjKBvRKPL5jxC5oBj1

[root@Matrix ~]# cat /boot/grub/grub.conf
default=0
timeout=2
splashimage=(hd0,0)/grub/splash.xpm.gz

hiddenmenu
password --md5 $1$3SoEu8O7$AmXQgjKBvRKPL5jxC5oBj1

title Red Hat Enterprise Linux (2.6.32-71.el6.x86_64)
root (hd0,0)
kernel /vmlinuz-2.6.32-71.el6.x86_64 ro root=LABEL=/ rhgb quiet i8042.reset
initrd /initramfs-2.6.32-71.el6.x86_64.img

{% endhighlight %}

**NOTE!** i8042.reset is extra argument which will fix Lenovo Y500 Laptop mouse problem
{: .notice}


#### 3. Kernel Initialization

##### Kernel Boot Time Function

* Device Detection
* Device Driver Initialization
* Mount The Root Filesystem - Read Only
* Load The Initial Process - `/sbin/init` With PID 1

* Kernel Boot Messages: `/var/log/dmesg`


##### Device Detection
* The Device Drivers compiled into the Kernel are called
And attempt to locate their corresponding Devices

##### Device Driver Initialization
* The Essential Drivers has been compiled as Modules
* The Modules Loaded from `initramfs-*.img`
* The Kernel Temporarily Mount These Modules On RAM Disk

##### Mount Root Filesystem
* After All The Essential Drivers Are Loaded,
The Kernel Will Mount The Root Filesystems As Read Only

##### Load The Initial Process
* Then The First Process - `/sbin/init` Is Loaded With PID 1
* Then The  Kernel Pass The Control To The `/sbin/init` Process
* The Init Is The Parent Of All Processes & You Can Verify That With `pstree` command


#### 4. Init Initialization

* In The Red Hat Enterprise Linux 6, The Upstart Init(8) Is The Event Based Daemon

* The Processes Managed By The Init Daemon Are Known As Jobs
* On Startup, Upstart Init(8) Daemon Reads Its Jobs Configuration From The `/etc/init/` Directory

  * Initialize Runlevel<br>
  * System Initialization<br>
  * Standalone Service Initialization<br>
  * Runlevel Specific Script Directories<br>
  * Spawn Gettys On Virtual Consoles<br>
  * Initialize X Server In Runlevel 5<br>


##### Initialize Runlevel

* The System First Check The `/proc/cmdline` File
* The `/proc/cmdline` File Contains The Kernel Arguments
Passed During System Bootup Time Via Grub Menu Editing Mode

* If The Runlevel Is Specified In The Kernel Arguments
Then The System Initialize The Specified Runlevel

* If The Runlevel Is Not Specified In The Kernel Arguments
Then The System Initialize The Initdefault Runlevel

* Default Runlevel:	`/etc/inittab`


##### System Initialization

* `/etc/rc.d/rc.sysinit`

* Activate udev
* Activate selinux
* Sets The Kernel Parameters- `/etc/sysctl.conf`

* Sets Hostname
* Activate RAID Devices
* Activate LVM  Devices

* Checking Filesystems
* Mounting The Local Filesystems
* Enabling The Local Filesystems Quotas
* Enabling The `/etc/fstab` Swaps Partition
* Remount The Root Filesystem Read-Write Mode

* Cleans Up State Locks And PID Files



##### Standalone Service Initialization

* `/etc/rc.d/rc`

* Initialize /etc/inittab Initdefault Runlevel
* Start/Stop Service Whenever Runlevel Changed

* Findout Previous & Current Runlevel
* First Kill Services In Runlevel Specific Scripts Directories
* Now Starts Services In Runlevel Specific Scripts Directories


##### Runlevel Specific Scripts Directories

`/etc/rc.d/rc[0-6].d/`


* `K*` Symbolic Link Called With A Stop Arguments
* `S*` Symbolic Link Called With A Start Arguments

* The System V Init Scripts Resides In: `/etc/rc.d/init.d/`
* Behaviour Configured With `/etc/sysconfig/`


**Non-Service Startup**

* `/etc/rc.d/rc.local`

* Common Place For Custome Modification
* Runs At The End Of Runlevel Specific Scripts Directories (S99local)

**Better Practice**

* Create A System V Init Script
* The `/etc/rc.d/init.d/` Scripts Can Be Used As A Starting Point
* The `/etc/rc.d/init.d/` Scripts Are Simple Case Statements, That Handle The 1st Arguments Of `status` `star` `stop` `restart`

**Three Addition Things To Do Within The Scripts**
<pre></pre>
**i) Modify The Comment Line To Support The chkconfig**

* `#chkconfig: <runlevel> <SS> <KK>`
* `#chkconfig: 2345 55 25`

**ii) Manage A File Named `/var/lock/subsys/servicename`**

* Whose Existence Declares That The Service Is Running
* And Used By The `/etc/rc.d/rc` Script To Determine Whether To Run The Symbolic Linked Scripts

**iii) Manage A File Named `/var/run/servicename.pid`**

* Store The PID Number Of The Service


**Transient Services**

* The xinetd Daemon Manages On Demand Services

* Service IP Redirection
* Host Based Authentication
* Service Statistics & Logging
* Less Frequently Needed Services
* Requiring Additional Resource Managements

* The xinetd Daemon Uses /etc/services For Port To Service Managements

**Configuration Files**

* `/etc/xinetd.conf` - Global Configuration
* `/etc/xinet.d/` - Override Global Configuration

**NEED MORE HELP LOOK AT HERE**

* `man 5 xinetd.conf`
* `/usr/share/doc/xinetd-*/`


##### Spawn Gettys On Virtual Consoles

* Configuration File:	`/etc/init/start-ttys.conf`

##### Initialize X Server In Runlevel 5

* Configuration File: `/etc/init/prefdm.conf`
