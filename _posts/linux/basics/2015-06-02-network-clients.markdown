---
layout: post
title: "Network Clients"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about network clients such as web-browsers, emails & messaging applications, SSH & FTP clients and network diagnostic tools"
tags: [Linux, Linux Basics, Network, SSH, FTP]
image:
  feature:
date: 2015-06-02T18:03:42+05:30
---

{% include _toc.html %}

### Web Clients

#### Links
* Links is a Text Based (Non GUI) Web Broser.
* Provided by the `elinks` package.
* Full support for Frames and SSL.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ links http://www.redhat.com
[mitesh@Matrix ~]$ links -dump http://www.redhat.com
[mitesh@Matrix ~]$ links -source http://www.redhat.com
{% endhighlight %}

#### Wget
* The `wget` is a non-interactive network downloader.
* Retrieves files via HTTP and FTP.

**Options:**
<pre>
/-----------------------------------------------------------------------------------------------\
|												|
|	--continue (-c)	|	Continue Partially Downloaded File				|
|												|
|	--tries=number	|	Retry the specified number of times to download the files	|
|	--wait=seconds	|	Wait the specified number of seconds between the retrievals	|
|												|
|	--recursive	|	Turn on recursive retrieving					|
|	--level=depth	|	Specify recursion maximum depth level				|
|			|	(The default maximum depth is 5)				|
|	--convert-links	|	After download is complete convert all links,			|
|			|	To make them suitable for local viewing				|
|												|
\-----------------------------------------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ wget http://www.redhat.com/training/index.html

[mitesh@Matrix ~]$ wget -c ftp://ftp.site.com/selinux.tar.gz

[mitesh@Matrix ~]$ wget --tries=50 --wait=30 ftp://ftp.site.com/files

[mitesh@Matrix ~]$ wget --recursive --level=1 --convert-links http://www.site.com
{% endhighlight %}

#### Google Chrome/Firefox
- Tabbed Browsing
* Popup Blocking
* Cookie Manageent
* Themes and Extensions
* Multi-Engine Search Bar
* Supports For Many Popular Plug-ins


### Email and Messaging

#### Graphical Mail Clients

##### Evolution
* Flexible Email & Groupware Tool
* Used For
  * Email Access
  * Maintain a Calendar, Tasklist and Contacts Database

##### Thunderbird
* Standalone Mozilla Email Client
* Used For
  * Email Access
  * Usenet and RSS Supports

**Security and Anti-spam Features:**

* Evolution and Thunderbird both support the Bayesian Spam Fileters.

* Evolution and Thunderbird both allows emails to be signed and encrypted.
* Evolution integrates supports for the GPG (GNU Privacy Guard) utility for this purpose.
* By Default, Thunderbird uses S/MIME for this purpose but use GPG with the help of the plugin called EnigMail.


#### Non-GUI Mail Clients

##### Mutt
* Mappable Hotkeys
* Highly Configurable
* Context Sensitive Help with ?
* Message Threading & Colorizing
* GNU Privacy Guard (GPG) Integration
* Supports POP, IMAP and LocalMailboxes

**Examples:**

* You Can Read One Mailbox (POP Account, IMAP Account or Local Mailbox) At A Time.
{% highlight bash %}
# Start The Local Mailbox
[mitesh@Matrix ~]$ mutt

# The mutt -f Starts The Specified Mailbox
[mitesh@Matrix ~]$ mutt -f imaps://user@server
{% endhighlight %}


**Note!:** You Can Change Which Mailbox Mutt Viewed By Default, By Altering Its Configuration File `~/.muttrc`.
{: .notice}
<br>



### Open SSH - Secure Remote Shell
* SSH Allows Remote Logins & Remote Command Execution Via A Secure Encrypted Connections.

**Syntax:**
{% highlight bash %}
ssh [user@]hostname
ssh [user@]hostname command
{% endhighlight %}

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ssh localhost
[mitesh@Matrix ~]$ ssh root@bluehost.com

[mitesh@Matrix ~]$ ssh neo@localhost 'df -h'
neo@localhost's password:

Filesystem            		Size  Used Avail Use% Mounted on
/dev/mapper/vg_Matrix-lv_root	 50G  2.4G   45G   6% /
tmpfs                 		495M  472K  494M   1% /dev/shm
/dev/sda1             		485M   30M  430M   7% /boot
/dev/mapper/vg_Matrix-lv_home	 59G  3.2G   53G   6% /home
{% endhighlight %}


#### scp - Secure File Transfer

* The `scp` works like `cp` command.
* The `scp` is a secure remote file copy program.

**Syntax:**
{% highlight bash %}
scp [OPTIONS] [[user@]host1:]file1... [[user@]host2:]file2

* Options:
-r(recursive):          Recursively copy an entire directory tree
-p(preserve):           Preserve the permissions, ownership, and time stamps
{% endhighlight %}

**Examples:**
{% highlight bash %}
# Download the process.log file
[mitesh@Matrix ~]$ scp neo@localhost:process.log .
neo@localhost's password:
process.log                                        100% 8843     8.6KB/s   00:00

# Upload the userstat.log file
[mitesh@Matrix ~]$ scp userstat.log neo@localhost:.
neo@localhost\'s password:
userstat.log                                        100% 8843     8.6KB/s   00:00
{% endhighlight %}

#### rsync - Efficient File Sync
* The `rsync` is a faster than `scp`.
* The `rsync` copies the difference in like files.
* The `rsync` uses a secure ssh connections for transport the files.
* The `rsync` is a fast, versatile, remote (and local) file-copying tool.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ rsync --progress neo@localhost:file1 .
neo@localhost's password:
file1
55 100%   53.71kB/s    0:00:00 (xfer#1, to-check=0/1)

sent 36 bytes  received 129 bytes  36.67 bytes/sec
total size is 55  speedup is 0.33

[mitesh@Matrix ~]$ cat file1
Hello Linux!
Welcome to the OSS!
{% endhighlight %}




#### Open SSH Keybased Authentication
* SSH Allows You To Authenticate Using The Private-Public Key Schema.
* Optional, Password less, But Still Secure Authentication

**Uses Two Keys Generated By The ssh-keygen**

1. Private Key
  * Private Key Stay On Your System
  * Usually Passphrase Protected (Recommended)

2. Public Key
  * Public Key Is Copied To The Destination With `ssh-copy-id`
  * By Default `ssh-copy-id` copy the `~/.ssh/id_rsa.pub`

**Syntax:**
{% highlight bash %}
ssh-copy-id [user@]hostname
{% endhighlight %}

**Examples:**
{% highlight bash %}
ssh-copy-id -i ~/.ssh/identity.pub [user@]hostname
ssh-copy-id -i ~/.ssh/id_rsa.pub [user@]hostname
ssh-copy-id -i ~/.ssh/id_dsa.pub [user@]hostname
{% endhighlight %}


**NOTE!:**	Older system may not have `ssh-copy-id` command <br>
Which will require you to manually copy your public keys to the destination system, <br>
And append it to the `~/.ssh/authorized_keys` file, or create the file if it does not exist. <br>
The `~/.ssh/authorized_keys` file must be readable by you. Otherwise ssh will ignore it.
{: .notice}
<br>

**Structure Of ssh-keygen**
<pre>
/---------------------------------------------------------------\
|								|
|				identity	Private	600	|
|			RSA1					|
|				identity.pub	Public	644	|
|								|
|	ssh-keygen						|
|								|
|				id_rsa		Private	600	|
|			RSA					|
|				id_rsa.pub	Public	644	|
|								|
|				id_dsa		Private	600	|
|			DSA					|
|				id_dsa.pub	Public	644	|
|								|
\---------------------------------------------------------------/
</pre>

**Examples:**
{% highlight bash %}
# Generate Protocol2 RSA Key Pairs
[mitesh@Matrix ~]$ ssh-keygen

# Generate Protocol2 DSA Key Pairs
[mitesh@Matrix ~]$ ssh-keygen -t dsa

# Generate Protocol1 Key Pairs
[mitesh@Matrix ~]$ ssh-keygen -t rsa1
{% endhighlight %}

**NOTE!:** By Default `ssh-keygen` Generates Protocol2 RSA Key Pairs.
{: .notice}
<br>



#### ssh-agent
* If you created your private key with a passphrase, which is highly recommended for the security reasons,
Then whenever the key is used to connect to the other machines, you must type your passphrase.

* An Authentication Agent Stores The Decrypted Private Keys
* Thus, Passphrase only need to be entered once.
* In GNOME Authentication Agent is provided automatically.
* Otherwise, You will need to create a Agent-Managed Shell with ssh-agent command

{% highlight bash %}
[mitesh@Matrix ~]$ ssh-agent bash
[mitesh@Matrix ~]$
{% endhighlight %}

* The Keys are added to the Agent with `ssh-add` command
* If no filename is specified, The Keys stored in `~/.ssh/id_rsa` and `~/.ssh/id_dsa` will be used
* You can view the list of stored keys by running

{% highlight bash %}
[mitesh@Matrix ~]$ ssh-add -l
{% endhighlight %}

**NOTE!:**	When you are finished, Just type exit to return your original shell. <br>
When the Agent is terminated, All your keys are forgotten.
{: .notice}
<br>

### FTP Clients
* Several FTP Clients are availables such as `sftp`, `lftp`, `lftpget`, `gftp` and many more.

#### sftp
* Interactive Secure File Transfer Program.
* The sftp uses a secure ssh connections for transport the files.

#### lftp:
* More Featureful File Transfer Program.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ lftp ftp.example.com
[mitesh@Matrix ~]$ lftp -u joe ftp.example.com
{% endhighlight %}

#### lftpget
* Non-interactive File Transfer Program.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ lftpget ftp://ftp.example.com/pub/file.txt
{% endhighlight %}

#### gftp
* Graphical File Transfer Program.
* `Applications -> Internet -> gFTP`
* Allows Drag and Drop Transfer
* Anonymous or Authenticated Access
* Optional Secure Transfer vis SSH (sftp)


### SMB Clients

* FTP Like Client To Access SMB/CIFS Resources On Server
* SMB Clients are used to access the shared resources on the MS Windows Network

**Options:**
{% highlight bash %}
-W:	Workgroup or Domain
-U:	Username
-N:	Suppress password prompt (Useful when accessing a service that does not require a password)
{% endhighlight %}

### File Transfer with Nautilus
* Nautilus are also used to access the remote file shares.
* Places -> Connect to Server
* Allows Drag and Drop File Transfer
* Graphically Browse With Multiple Protocols
* Suported Connection Types:
* SSH
* FTP
* SFTP
* WebDAV (HTTP)
* Secure WebDAV (HTTPS)
* Microsoft Windows Shares

**Nautilus URL**
{% highlight text %}
ftp://ftp.example.com		|	Connects via ftp to ftp.example.com
sftp://user@ftp.example.com	|	Same as above, but connecting securely via ssh as user

smb://				|	List all available SMB Servers
smb://example			|	List SMB Share on example server
smb://user@example		|	Same as above but authenticate as user
{% endhighlight %}

### Xorg Clients
* One Of The Most Important Features Of Xorg Is Its Client/Server Architecture,
That Makes Any Graphical Application On Linux System Completely Network Transparent.

* That Means You Can Start Any Graphical Application On The Remote System & That Application Displayed On The Local System.
* The Graphical Application Running In This Way
* Affects Files On The Remote System
* Take Up Resources (Cpu Cycles, Memory, Etc) On The Remote System
* And That Graphical Application Only Displayed On The Local System

**Note!:** All Graphical Applications Are X Clients & Connects To The Remote X Server Via TCP/IP <br>
By Default, The Data Transfer Is Not Encrypted But Can Be Tunneled Securely Over An SSH Connection
{: .notice}

{% highlight bash %}
ssh -X [user@]hostname xterm &
{% endhighlight %}


### Network Diagnostic Tools

#### ping
* Detects If It Is Possible To Communicate With Other Systems.
* Many System No Longer Respond To The Pings.

#### host
* DNS Lookup Utility.
* It is normally used to convert Hostnames to IP addresses and vice versa.

#### dig
* DNS Lookup Utility.
* Similar to host but greater in details.

#### netstat
* Provides a Number Of Network Statistics.

#### traceroute
* Display The List Of Computers(IP Addresses) Through Which The Packet Is Passed To Reach Their Destination.
