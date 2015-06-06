---
layout: post
title: "Package Management"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about manage package using yum and rpm commands"
tags: [Linux, Basics, Package, Yum, RPM]
image:
  feature:
date: 2015-06-06T16:15:25+05:30
---

{% include _toc.html %}

### Software as Packages

**Syntax:**
{% highlight bash %}
package-version-release.arch.rpm
{% endhighlight %}

* package:	The name of the package
* version:	The upstream developer version
* release:	The package changes (bugfixes/backports)
* arch:		The processor architecture of binaries

**Contains:**

* Summary, Description, Changelog
* Files Archive: Binaries, Libraries, Default Configuration, Documentation
* Instructions:	 Dependencies, Pre/Post Install/Uninstall
* Signature


### rpm command

#### Primary RPM Options

* `-i | --install`		Install A New RPM Packages

* `-F | --freshen`		Upgrade The Currently Installed Packages To A Newer Version
Skipped If Older Version Is Not Available.

* `-U | --upgrade`		Upgrade The Currently Installed Packages To A Newer Version
Simply Install The Package If Older Version Is Not Available

* `-e | --erase`		Erase Or Remove The Packages

* `-q | --query`		Query The RPM Packages

* `-V | --verify`		Verify The RPM Packages
File Type, Permissions, Owner, Group, File Size, MD5 Checksum, Modify Time Against The RPM Database


#### General Options

* `-v`  Print The Verbose Information
* `-h | --hash`	Print The Hash Mark Progress Bar Used With `-v` Options

#### Install Options

* `--aid`		Also Install Dependencies
* `--nodeps`	Don’t do a dependency check before installing or upgrading a package
* `--oldpackage`	Allow an upgrade to replace a newer package with an older one
* `--replacepkgs`	Install the packages even if some of them are already installed on the system
* `--replacefiles`	Install the packages even if they replace files from other, already installed, packages

#### Query Options

* `-a | --all`	Query All Installed Packages
* `-i | --info`	Display The Package Information - Name Version & Description
* `-l | --list`	List Files Inside The RPM Packages
* `-f | --file`	Query To Findout Which Package Owning The Specified File
* `-p | --package`	Query An Pre-Installed Packages

  * `--last`		List The Installed Packages (Latest First)
  * `--changelog`	Display Changelog Information
  * `--scripts`		List The Scripts That Are Used For Installation & Uninstallation Process

* `-c | --configfiles`	List The Configuration Files
* `-d | --docfiles`		List The Documentation Files
* `-R | --requires`		List The Capabilities On Which This Package Depends


**Examples:**
{% highlight bash %}
[root@Matrix ~]# rpm -ivh firefox-3.6.9-2.el6.x86_64.rpm
[root@Matrix ~]# rpm -Fvh firefox-3.7.19-2.el6.x86_64.rpm
[root@Matrix ~]# rpm -Uvh mysql-5.1.47-4.el6.x86_64.rpm

[root@Matrix ~]# rpm -e firefox


[root@Matrix ~]# rpm -qa		[root@Matrix ~]# rpm -qa '*irefo*'
[root@Matrix ~]# rpm -qi firefox
[root@Matrix ~]# rpm -ql firefox

[root@Matrix ~]# rpm -qf /etc/passwd	[root@Matrix ~]# rpm -q --whatprovides /etc/passwd
setup-2.8.14-10.el6.noarch		setup-2.8.14-10.el6.noarch

[root@Matrix ~]# rpm -qf /bin/bash	[root@Matrix ~]# rpm -q --whatprovides /bin/bash
bash-4.1.2-3.el6.x86_64			bash-4.1.2-3.el6.x86_64


[root@Matrix ~]# rpm -qip httpd-2.2.15-5.el6.x86_64.rpm
[root@Matrix ~]# rpm -qlp httpd-2.2.15-5.el6.x86_64.rpm

[root@Matrix ~]# rpm -qa 'kernel*'
kernel-firmware-2.6.32-71.el6.noarch
kernel-2.6.32-71.el6.x86_64

[root@Matrix ~]# rpm -qa 'kernel*' --queryformat "%{NAME}-%{VERSION}-%{RELEASE}.%{ARCH}\n"
kernel-firmware-2.6.32-71.el6.noarch
kernel-2.6.32-71.el6.x86_64


[root@Matrix ~]# rpm -qa --last		[root@Matrix ~]# rpm -qa --last | tac
[root@Matrix ~]# rpm -q --changelog setup	[root@Matrix ~]# rpm -q --scripts setup
[root@Matrix ~]# rpm -qc setup			[root@Matrix ~]# rpm -qd setup			[root@Matrix ~]# rpm -qR setup



# Verify All Installed Packages Against The RPM Database
[root@Matrix ~]# rpm -Va
{% endhighlight %}

### Trusted Installation

* Red Hat Signs All The Package Files With GPG Private Signature.
* The Red Hat Public Key Is Provided With Every Red Hat Distribution.
* To Verify The Integrity Of Any Package File, You Must 1st Import The Red Hat Public Key.
* The RPM Utility Will Automatically Verify The Signature Of Any Package You Install At The Install Time.

**Examples:**
{% highlight bash %}
[root@Matrix ~]# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

# To Query The List Of Public Keys That You Have Been Imported
[root@Matrix ~]# rpm -qa gpg-pubkey
gpg-pubkey-2fa658e0-45700c69
gpg-pubkey-fd431d51-4ae0493b

# To Verify The Signature Of A RPM Package
[root@Matrix Packages]# rpm -K firefox-3.6.9-2.el6.x86_64.rpm
[root@Matrix Packages]# rpm --checksig firefox-3.6.9-2.el6.x86_64.rpm
firefox-3.6.9-2.el6.x86_64.rpm: rsa sha1 (md5) pgp md5 OK
{% endhighlight %}


### About Yum

* YUM: Yellowdog Update Modified
* Command-Line Frontend To The RPM
* Designed To Resolve Package Dependencies
* Locate Packages Across The Multiple Repositories
Such As RHN, Satellite Or Proxy Servers & Private FTP/Http Repository Servers


**Managing Packages With Yum**

#### Primary Yum Options

* `install`	Install A Latest Version Of The Package While Ensuring That All Dependencies Are Satisfied

* `update`		If Run Without Any Packages, Update Every Currently Installed Packages.
If One Or More Packages Or Package Globs Are Specified, Yum Will Only Update The Listed Packages.
While Updating Packages, Yum Will Ensure That All Dependencies Are Satisfied.

* `remove`		Remove The Specified Packages From The System
As Well As Removing Any Packages Which Are Depend On The Specified Packages

* `groupinstall`	Install All Of The Individual Packages In The Group

* `localinstall` 	Install A Set Of Local RPM Files
If Required The Enabled Repositories Will Be Used To Resolve Dependencies


* `info`		Display The Package Information - Name Version & Description
* `list`		List The Installed & Available Packages

* `groupinfo`	Display The Group Information -
* `grouplist`	List The Installed & Available Groups

* `provides`	Findout Which Package Owning The Specified File
* `serach`		Search Any Packages By Matching A String In The Package Name, Description And Summary Fields


**Examples:**
{% highlight bash %}
[root@Matrix ~]# yum install firefox
[root@Matrix ~]# yum install -y httpd

[root@Matrix ~]# yum update
[root@Matrix ~]# yum update firefox
[root@Matrix ~]# yum update [package1] [package2] [...]

[root@Matrix ~]# yum remove firefox

[root@Matrix ~]# yum groupinstall "GNOME Desktop Environment"

[root@Matrix Packages]# yum localinstall elinks-0.12-0.20.pre5.el6.x86_64.rpm

[root@Matrix ~]# yum list
[root@Matrix ~]# yum list all
[root@Matrix ~]# yum list updates
[root@Matrix ~]# yum list available
[root@Matrix ~]# yum list "*irefo*"
[root@Matrix ~]# yum list installed

[root@Matrix ~]# yum list installed kernel\*

[root@Matrix ~]# yum info firefox

[root@Matrix ~]# yum grouplist
[root@Matrix ~]# yum grouplist all
[root@Matrix ~]# yum groupinfo "GNOME Desktop Environment"

[root@Matrix ~]# yum provides /etc/passwd
setup-2.8.14-10.el6.noarch : A set of system configuration and setup files

[root@Matrix ~]# yum whatprovides /etc/passwd
setup-2.8.14-10.el6.noarch : A set of system configuration and setup files

[root@Matrix ~]# yum search firefox
{% endhighlight %}



#### Enabling Private Yum Repositories

* Default Settings:	`/etc/yum.conf`
* Personal Settings:	`/etc/yum.repos.d/[name].repo`
* Template Settings:	`/etc/yum.repos.d/rhel-debuginfo.repo`

**Syntax:**
{% highlight bash %}
[repositoryid]
name=Some Name For This Repository
baseurl=url://path/to/repository/
enabled=0|1
gpgcheck=0|1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
{% endhighlight %}

**NOTE!:**	`[repositoryid]` Must be a unique name for each repository, one word.
Look at `yum.conf(5)` and the `[repositories]` options for more details.
{: .notice}
<pre></pre>

**Example:**
{% highlight bash %}
[root@Matrix ~]# cat  /etc/yum.repos.d/rhel-debuginfo.repo
[rhel-debuginfo]
name=Red Hat Enterprise Linux $releasever - $basearch - Debug
baseurl=ftp://ftp.redhat.com/pub/redhat/linux/enterprise/$releasever/en/os/$basearch/Debuginfo/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
{% endhighlight %}


#### Private Yum Repositories

1. Create A Directory That Can Hold Your Packages.
{% highlight bash %}
mkdir -p /var/ftp/pub/RHEL/6Server/x86_64/Packages
{% endhighlight %}

2. Make That Directory Available Via FTP or HTTP.
{% highlight bash %}
yum install vsftpd-2.2.2-6.el6.x86_64.rpm
chkconfig vsftpd on
service vsftpd start
{% endhighlight %}

3. Install The createrepo RPM.
{% highlight bash %}
yum install createrepo-0.9.8-4.el6.noarch.rpm deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm python-deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm
{% endhighlight %}

4. Run following command
{% highlight bash %}
createrepo -v /dir/packaagedir
cd /var/ftp/pub/RHEL/6Server/x86_64/Packages
createrepo -v .
{% endhighlight %}

`primary.xml.gz` List Of All The RPM In The Repository As Well As Dependency Information & List Of Files Inside The RPM Packages This is used by `rpm -qlp`
<br>
`filelists.xml.gz`	List Of All The Files In All The RPMs
This is used by yum provide
<br>
`other.xml.gz`		Additional Information, Including The Chanage Logs For The RPMs
<br>
`repomd.xml`		Contains The Timestamps & Checksum Values For The Above 3 Files
The System Refresh The Cache If repomd.xml File Indicates The Repository Has Been Changed.
<br>
`comps*.xml`		This Optional File Contains The Information About Group Packages.
Used For Group Installations & Dependency Resolution


**NOTE!:**	Repository Information Is Cached
`yum clean dbclean | all`
{: .notice}

### Install A New Kernel

* Installing a new kernel is requires a little more thought and caution.
* Installing a new kernel is one of the few things that requires a reboot of the system.

* Kernels Are Installed In Parallel, Not Upgraded:

  * Do Not Use `rpm -F` & `rpm -U`
  * Rather Than Use `rpm -i`

* Yum Properly Handles The Kernel,
* Yum Perform An Install Rather Than Update When Working With A Kernel,
Whether You Use `yum update` or `yum install`


**NOTE!:** The `rpm -F` & `rpm -U` Options Removes The Older Version Of The Packages<br>
So If Your Newly Installed Kernel Are Unstable Then You Could Be Left With Unbootable System.<br>
When You Run An Install Instead Of An Upgrade, The Older Version Of The Kernel Is Still Available,
And Can Be Selected From The Bootloader.
<br><br>
All Of The Kernel Files Are Version Specific,
So It Is Possible To Install Multiple Kernels In A Single System.
<br><br>
By Default, The New Kernel Is Automatically Added To The GRUB Bootloader & Make It Default.
You Can Change This Behaviour By Editing `/etc/sysconfig/kernel` File.
<br><br>
If You Accidentally Remove The Kernel, Then Use `rpm -i --oldpackage` To Get The Older Kernel Package.
{: .notice}


### About The Red Hat Network
* The Red Hat Network Allows The Administrators
* To Manage Software Installation & Upgrades Efficiently
* Using A Combination Of Your RHN Acoount & The Yum Utility.

* The Red Hat Network Service Is Provided By
  * The rhn.redhat.com
  * The Local Satellite Or Proxy Server

* Features Of Red Hat Network:
  * Web Based Management Interface
  * Uses HTTPS for All The Transactions
  * Centralized Platform For System Management

#### Red Hat Network Client

**Registration**

* The RHN Registration Is Usually Done At The Installation Time - FirstBoot
* The rhn_register Will Interactively Create Configuartion Files For The Yum Utility
As Well As Register Your System At RHN Or Satellite Or Proxy Servers.
* The RHN Registration Can Be Automated With The Help Of rhnreg_ks command.

**Interactive Usage:**

* Yum Uses Plug-in For RHN Communication
* Already Configured In ``/etc/yum/pluginconf.d/rhnplugin.conf`

**Remote Management:**

* Actions Queued On RHN Server
* The rhn_check Polls RHN Server Immediately
* The rhnsd Polls RHN Server Every 4 Hours (``/etc/sysconfig/rhn/rhnsd`)


### PackageKit
<pre>
-------------------------------------------------------------------------------------------------------------------------
|		Function				How To Open				Shell Command		|
-------------------------------------------------------------------------------------------------------------------------
|	Install/Remove			System -> Administration -> Add/Remove Software		gpk-application		|
|	View Package Information											|
|-----------------------------------------------------------------------------------------------------------------------|
|	Perform Package Update		System -> Administration -> Software Update		gpk-update-viewer	|
|-----------------------------------------------------------------------------------------------------------------------|
|	Enable/Disable			System -> Administration -> Add/Remove Software		gpk-repo		|
|	Yum Repository			System -> Software Sources							|
|-----------------------------------------------------------------------------------------------------------------------|
|	View The Transaction		System -> Administration -> Add/Remove Software		gpk-log			|
|	Information			System -> Software Log								|
|-----------------------------------------------------------------------------------------------------------------------|
|	Alert When Update		System -> Preferences -> Startup Applications		gpk-update-icon		|
|	Available			Startup Program -> PackageKit Update Applet					|
|-----------------------------------------------------------------------------------------------------------------------|
|	Set The Packagekit									gpk-prefs		|
|	Preferences													|
-------------------------------------------------------------------------------------------------------------------------
</pre>
