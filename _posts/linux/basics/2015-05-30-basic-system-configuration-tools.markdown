---
layout: post
title: "Basic System Configuration Tools"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about Network, DNS and Printer Configuration"
tags: [Linux, Basics, Network, DNS, Printer]
image:
  feature:
date: 2015-05-30T14:09:28+05:30
---

{% include _toc.html %}

### 1. Network Configuration

* Most networks use TCP/IP as the basic protocols for network communications. Regardless of the operating system you use.
* Certain TCP/IP settings always need to be configured for your system to communicate with other systems.


#### Important Network Settings
* Device Activation
* IP Configuration
* Default Gateway
* DNS Configuration

#### Basics Of Network Setting

* Network Interface Card's IP Address
* Subnet Mask
* Network Number

* In general these settings and more will be provided automatically by your network's Dynamic Host Configuration Protocol (DHCP) Server.
* Even when manual configuration is required, often the IP Address, which is used to identify your host,
is the only one of these that needs to be set explicitly.

* If you are not using DHCP Server, you will also need to specify:
* Default Gateway
* DNS Server

* The Default Gateway is the IP Address of the Device or System to which Communications Destinated for Hosts on another Network.
* It is the job of the gateway to see that such messages reach their intended destinations.

* The DNS Server is used to translate Domain Names like `google.com` to IP Addresses like `216.58.196.110`


#### Managing Ethernet Connections

* Network interfaces are named sequentially:	`eth0`, `eth1`, `wlan0`
* Multiple addresses can be assigned to a device with aliases
* Aliases are labeled:	`eth0:1`, `eth0:2`, etc
* Aliases are treated like separate interface

* Every system has a special network interface called lo (LocalHost or LoopBack) with `127.0.0.1` IP Address.

##### Network Interface Configuration
{% highlight bash %}
ifconfig [interface]
{% endhighlight %}

**NOTE!:** By Default, `ifconfig` command print the information about all active interfaces. <br>
If the interface name is passed as an argument, `ifconfig` command print the information about that interface only.
{: .notice}
<br>
**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ ifconfig eth0
eth0	Link encap:Ethernet  HWaddr 00:09:6B:CD:2B:87
inet addr:192.168.0.254  Bcast:192.168.0.255  Mask:255.255.255.0
inet6 addr:fe80::209:6bff:fecd:2b87/64  Scope:Link
UP BROADCAST RUNNING MULTICAST   MTU:1500  Metric:1
RX packets:851525 errors:0 dropped:0 overruns:0 frame:0
TX packets:1132322 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1000
RX bytes:211140434 (201.3 MiB)  TX bytes:1113058956 (1.0 GiB)
{% endhighlight %}

##### Enable/Disable Network Interface
{% highlight bash %}
ifup ethX;	ifconfig ethX up
ifdown ethX;	ifconfig ethX down
{% endhighlight %}


##### Graphical Network Configuration:

`System -> Preferences -> Network Connections`

* Add, Edit, Delete Network Interface
* Assign IP Addresses/DHCP
* Modify Gateway Address
* Modify DNS Setting

**NOTE!:** The `system-config-network` also provides all the features listed above.
{: .notice}
<br>

#### Network Configuration File

##### Ethernet Devices

* Whether or not you use `system-config-network` to configure your network interfaces,
All the Network Interface settings are stored in `/etc/sysconfig/network-scripts/` directory.
* The Network Interface Configuration is stored in a Text Files
`/etc/sysconfig/network-scripts/ifcfg-ethX`

**NOTE!:**	These files are read by `system-config-network`, `ifup`, `ifdown` and other tools that bring the Network Interface up and down.
{: .notice}
<br>

**Example:**
<pre>
/-------------------------------------------------------------------------------\
|	Dynamic Configuration		|	Static Configuration		|
|--------------------------------------------------------------------------------
|										|
|	DEVICE=ethX			|	DEVICE=ethX			|
|	HWADDR=00:09:6B:CD:2B:87	|	HWADDR=00:09:6B:CD:2B:87	|
|	BOOTPROTO=dhcp			|	IPADDR=192.168.0.123		|
|	ONBOOT=yes			|	NETMASK=255.255.255.0		|
|	TYPE=Ethernet			|	GATEWAY=192.168.0.254		|
|					|	ONBOOT=yes			|
|					|	TYPE=Ethernet			|
\-------------------------------------------------------------------------------/
</pre>

**NOTE!:** The Network Interface file is just a collection of bash variables that define the interface's setting.
Remember the syntax of bash variable is `VARIABLE=VALUE`, with no spaces on either side of the =.
{: .notice}
<br>

#### Network Interface Configuration
<pre>
/-----------------------------------------------------------------------------------------------------------------------\
|	Setting		|	Meaning											|
|------------------------------------------------------------------------------------------------------------------------
|															|
|	DEVICE		|	Specify the Network Interface Name or Alias (Example eth0)				|
|															|
|	HWADDR		|	Hardwaare(MAC) Address.									|
|			|	This setting is optional & can cause problems,						|
|			|	when ethernet card is replaced								|
|															|
|	BOOTPROTO	|	Where IP Settings should be retrieved from.						|
|			|	Set to dhcp to use DHCP.								|
|			|	Leave the variable unset or set it to static for manual IP Settings.			|
|															|
|	IPADDR &	|	Basic IP Settings.									|
|	NETMASK		|	Only necessary when not using the DHCP Server.						|
|															|
|	GATEWAY		|	Only necessary when not using the DHCP Server.						|
|			|	The Gateway can also be set in /etc/sysconfig/network file.				|
|			|	If the Gateway is defined in the /etc/sysconfig/network and ifcfg file,			|
|			|	The Gateway defined in the most recently activated ifcfg file is used.			|
|															|
|	ONBOOT		|	Whether to bring the network interface up automatically when the system boots.		|
|			|	Set to yes or no. The default value is no.						|
|															|
|	USERCTL		|	Whether to allow non-root users to bring this interface up & down.			|
|			|	Set to yes or no. The default value is no.						|
|															|
|	TYPE		|	Specify the type of network interface.							|
|															|
\-----------------------------------------------------------------------------------------------------------------------/
</pre>

#### Global Network Setting

* Some Network Settings are defined globally, rather than on a per-interface basis.
* The Global Network Settings are defined in the `/etc/sysconfig/network`

* Many may be provided by DHCP Server.
* GATEWAY can be overridden in `ifcfg` file.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=server1.example.com
GATEWAY=192.168.2.254
{% endhighlight %}

**NOTE!:** If the Gateway is defined in the `/etc/sysconfig/network` and `ifcfg` file,
The Gateway defined in the most recently activated `ifcfg` file is used.
{: .notice}
<br>

#### DNS Configuration

* Domain Name Service Translate Hostname to IP Address.
* The DNS Server address is specified by DHCP or in `/etc/resolv.conf`
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/resolv.conf
search example.com cracker.org
nameserver 192.168.0.254
nameserver 192.168.1.254
{% endhighlight %}

##### Explain DNS
* When we type `www.google.com` into web browser, it initiates a DNS Lookup,
in which it ask Local DNS Server what IP Address is assigned to that name.
* If Local DNS Server does not know, it will try to find out which other
DNS Server on the internet know about the `google.com` and will forward my
request to one of them.

##### Local DNS Server

* Local DNS Configuration is performed using the `/etc/resolv.conf`
* Remember two things while creating or modifying `/etc/resolv.conf` file

**search**

* The search specifies the Domain Name when incomplete DNS Name is give to the command.

* If `/etc/resolv.conf` file contains the line: `search example.com cracker.org`
* Now when we try to run following command `ping server1`
* In this case system automatically convert `server1` into `server1.example.com`
* If the `server1.example.com` is not found then system try `server1.cracker.org`

**nameserver**

* The nameserver is most important, as it specify the IP Address of the DNS Server that your system should use.
* Multiple nameserver are added but remember that nameserver are tried in order,
So be sure to put the fastest and available namesserver at first place.






### 2. Printing in Linux

* Printing on Linux system is handled by the Common Unix Printing System (CUPS).
* Printers May Be
* Local (serial, parallel or usb)
* Networked

#### Supported printer connections

* Local (serial, parallel or usb)
* Unix/Linux print server
* Netware print server
* Windows print server
* HP JetDirect

#### Queues
* Print request are sent to the queues.
* Different queues for the same printer may have different priority or output options.
* Queued jobs are sent to the printer on a First Come First Served basis.

#### Jobs
* Once a file has been sent to a queue for printing, it is called a job.
* Jobs may be canceled before or during printing.


#### system-config-printer
* A Graphical Configuration Tool is used to adding new printers to your systems.
* To run this tools
Select `System -> Administration -> Printing`	OR
Run the `system-config-printer` on the terminal

#### Printing Commands

##### lpr command
* `lpr` Send job to the printer
* Accepts ASCII, PDF, PostScript and other Formats
* Most applications under Linux output PostScript Formats

**Options**

* `-P`:	Select the specific printer.
* `-#`:	Set the number of copies to print from 1 to 100.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ lpr reports.txt

# Print 5 copies of the file reports.txt on the accounting printer
[mitesh@Matrix ~]$ lpr -P accounting -#5 reports.txt
{% endhighlight %}

##### lpq command

* `lpq` Show printer queue status.

**Options**

* `-P`:	Select the specific printer.

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ lpq

Printer:	ps@localhost
Queue:		no printable jobs in queue
Server:		no server active
Status:		job  'jay@localhost+916'  removed at 12:16:03.083
Rank	Owner/ID		Class	Job	Files		Size	Time
done	jay@localhost+185	  A	185	results		2067	08:38:04
{% endhighlight %}

##### lprm command

* `lprm`:	Removes a job from the print queue.

**Options**

* `-P`:	Select the specific printer.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ lprm 916
Printer ps@localhost:
{% endhighlight %}

* In this example, `lprm` responds with the name of the queue from which the job was removed.

**NOTE!:** A user may only remove his own jobs from the queue.
{: .notice}
<br>

##### lpstat command

* `lpstat`:	Prints cups status information.

**Options**

* `-a`:	List Configured Printers.


#### Printing Utilities

* `evince`:		Views PDF Documents
* `ps2pdf`:		Converts PostScript to PDF
* `pdf2ps`:		Converts PDF to PostScript. Which makes it easy to print PDF Documents right from the command line.
* `pdftotext`:	Converts PDF to Plain Text
* `enscript`, `a2ps`:	Converts Text to PostScript
* `mpage`:		Prints multiple pages per sheet


### 3. System's Date and Time

#### GUI Method

* `system-config-date`
`System -> Administration -> Date & Time`

* Can set Date/Time Manually or use NTP
* Additional NTP Servers can be added
* Can use Local Time or UTC

#### CLI Method

* date [MMDDhhmm[[CC]YY][.ss]]

**Examples:**
{% highlight bash %}
[root@Matrix ~]# date 01011010		# 1st  Jan, at 10:10am.
[root@Matrix ~]# date 01010101.01	# 1st  Jan, at 01:01:01 am.
[root@Matrix ~]# date 12312359		# 31st Dec, at 11:59pm.
[root@Matrix ~]# date 123123592007	# 31st Dec 2007, at 11:59pm.
[root@Matrix ~]# date 123123592007.05	# 31st Dec 2007, at 11:59:05pm.
{% endhighlight %}

### 4. Shell Scripting

* Taking Input With Positional Parameters:
* Many commands and scripts can perform different tasks depending on the arguments supplied to the program.
* Positional Parameters are special variables that hold the command-line arguments to the script.
* The available Positional Parameters are:
$0, $1, $2, $3, $4, $5, $6, $7, $8, $9, ${10}, ${11}...

**NOTE!:**	`$0`	Specify the program name<br>
`$*`	Holds all command-line arguments<br>
`$#`	Holds the number of command-line arguments
{: .notice}
<br>

**Examples:**
{% highlight bash %}
#!/bin/bash
# positionaltester
# Demonstrates the use of positional parameters

echo "The program name is $0"
printf "The first argument is %s and the second is %s\n" $1 $2
echo -e "All command line parameters are $*\n"
echo -e "Total number of command line parameters are $#\n"
{% endhighlight %}

{% highlight bash %}
[mitesh@Matrix ~]$ ./positionaltester Red Hat Enterprise Linux
The program name is ./positionaltester
The first argument is Red and the second is Hat
All command line parameters are Red Hat Enterprise Linux

Total number of command line parameters are 4
{% endhighlight %}

#### read command
* Taking Input With Read Command
* The read commands takes a line from the STDIN and breaks it down into individual words.
(Usaually a word is defined as a character string surrounded by white space such as spaces and tabs).

* The way the shell interprets  words may be changed by setting the `IFS` variable.

**NOTE!** IFS=':' will tells the shell that words are separated by colons instead of white space).
{: .notice}
<br>

* The 1st word is assigned to the 1st variable
The 2nd word is assigned to the 2nd variable and so on.

* If there are more words than variables, All the remaining words are assigned to the last variable.

**Options**

* `-p`:	Designates prompt to display.

**Examples:**
{% highlight bash %}
#!/bin/bash
echo -n "Enter name ( first last ): "
read FIRST LAST
echo "Your first name is $FIRST and your last name is $LAST"

#!/bin/bash
read -p "Enter name ( first last ): " FIRST LAST
echo "Your first name is $FIRST and your last name is $LAST"

#!/bin/bash
read -p "Enter several values: " value1 value2 value3
echo "value1 is $value1"
echo "value2 is $value2"
echo "value3 is $value3"
{% endhighlight %}
