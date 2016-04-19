---
layout: post
title: "Investigating and Managing Processes"
author:
modified: 2015-06-09T10:58:23+05:30
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about process management/scheduling, signals and job control"
tags: [Linux, Linux Basics, PS, JOB, AT, CRON, CRONTAB]
image:
  feature:
date: 2015-05-31T14:30:19+05:30
---

{% include _toc.html %}

### 1. Process
* A process is a set of instructions loaded into memory.
* Numeric Process ID (PID) used for identification.
* UID, GID and SELinux context determines filesystem access.
* The Linux Kernel tracks every aspect of a process by its PID under `/proc/PID`.

#### Listing Process
* The `ps` command is used to view the process information.
* By Default, shows processes from the current terminal

**Options**

* `a`:	Shows processes from all the terminals.
* `x`:	Shows all the processes owned by you, or shows all the processes when used together with the a option (such as: `ps ax`) Including processes that are not controlled by a terminal
Such as Daemon processes, This shows up as `?` in the tty column of the output

* `u`:	Shows process owner information.
* `f`:	Shows process parentage

* `o`:	Shows custom information
Such as pid, tty, stat, nice, %cpu, %mem, time, comm, command, euser, ruser

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ps
[mitesh@Matrix ~]$ ps a
[mitesh@Matrix ~]$ ps x
[mitesh@Matrix ~]$ ps u
[mitesh@Matrix ~]$ ps f
[mitesh@Matrix ~]$ ps xo pid,tty,stat,%cpu,%mem,time,command,euser,ruser
[mitesh@Matrix ~]$ ps axo pid,tty,stat,%cpu,%mem,time,command,euser,ruser
{% endhighlight %}

#### Process Status

* Every process has a state property,
which describes whether the process is actively using the cpu (Running),
in memory but not doing anything (Sleeping),
waiting for a resource to become available (Uninterruptable Sleep)
or terminated but not flushed from the process list (Zombie).
* Running and Sleeping are normal,
but the presence of Uninterruptable Sleep or Zombie processes may indicate problems lurking on your system.


**Uninterruptable Sleep**

* Process is sleeping and can not be woken up until an event occurs.
* It can not be woken up by a signal.
* Typically, the result of I/O operations, such as a failed network connections (For NFS Hard Mounts).

**Zombie**

* Just before a process dies, it sends a signal to its parent and waits for an acknowledgment before terminating.
* Even if the parent process does not immediately acknowledge this signal,
all resources except for the process identity number (PID) are released.
* Zombie processes are cleared from the system during the next system reboot
And do not adversely affect system performance.


#### Finding Process
{% highlight bash %}
# Most Flexible
[mitesh@Matrix ~]$ ps axo pid,tty,comm | grep 'cups'
1516 ?        cupsd
3066 pts/1    eggcups

# By predefined patterns: pgrep
[mitesh@Matrix ~]$ pgrep -U root
[mitesh@Matrix ~]$ pgrep -G mitesh

[mitesh@Matrix ~]$ pgrep cups
1516
3066

# By exact program name:	pidof
[mitesh@Matrix ~]$ pidof cupsd
1516
{% endhighlight %}


### 2. Signals
* Signals are simple messages that can be communicated to processes with commands like `kill`.
* Sent directly to processes, no user interface required.
* Programs associate actions with each signal.

* Signals are specified by name or number when sent `man 7 signals` shows complete list

* Signal 1	HUP  (SIGHUP)	Re-read Configuration Files
* Signal 9	KILL (SIGKILL)	Terminate Immediately
* Signal 15	TERM (SIGTERM)	Terminate Cleanly
* Signal 18	CONT (SIGCONT)	Continue If Stopped
* Signal 19	STOP (SIGSTOP)	Stop Process



#### Sending Signals to Process
* By PID:	`kill	[signal] pid ...`
* By Pattern:	`pkill	[signal] pattern`
* By Name:	`killall	[signal] command ...`

* `kill` can send many signals, but processes only respond to those signals whose they have been programmed to recognize.
* For Example: Most services are programmed to reload their configuration when they receive a HUP(1) signals.

* Some processes are terminated when they completed their tasks.<br>
Interactive applications may need the user to issue a quit command. <br>
In other cases, processes may need to be terminated with Ctrl+c, which sends an INT(2) signal to the process.

* The process is shutdown cleanly means
Terminate child process first &
Complete any pending I/O operations.

**NOTE!:** The KILL(9) signal should be used only if a process will not respond to a Ctrl+c or a TERM(15) signals.
Using KILL(9) signal on a routine basis may cause zombie processes and lost data.
{: .notice}
<br>
{% highlight bash %}
# The following are all identical and will send default TERM(15) signal to the process with PID number 3705
[mitesh@Matrix ~]$ kill 3705
[mitesh@Matrix ~]$ kill -15 3705
[mitesh@Matrix ~]$ kill -TERM 3705
[mitesh@Matrix ~]$ kill -SIGTERM 3705
{% endhighlight %}


### 3. Scheduling Priority

* Every running process has a scheduling priority:
A ranking among running processes determining which should get the attention of the processor.

* Priority is affected by a process' nice value.
* The nice value range from -20 to 19 ( Default is 0 )
  * -20:	Highest CPU
  * 19:	Lowest  CPU

#### Altering Scheduling Priority

* Niceness value may be altered...

* When starting a process
{% highlight bash %}
[mitesh@Matrix ~]$ nice -n 5 command
{% endhighlight %}
* After Starting the process
{% highlight bash %}
[mitesh@Matrix ~]$ renice 5 -p PID
{% endhighlight %}

**NOTE!:** 	Only root may decrease nice value.
Non-privileged users start a process at any positive nice value but cannot lower it once raised.
{: .notice}

{% highlight bash %}
[mitesh@Matrix ~]$ nice -n 10 myprog
[mitesh@Matrix ~]$ renice 15 -p PID
[root@Matrix ~]# renice -19 -p PID
{% endhighlight %}


#### Process Management Tools

##### CLI - top, htop
* Display list of processes running on your system, updated every 3 seconds.
* You can use keystrokes to kill, renice and change the sorting order of processes.
* Use ? key to view the complete list of hotkeys.
* You can exit top by pressing the q key.

##### GUI - gnome-system-monitor
* The gnome-system-monitor, which can be run from the console
Or by selecting Applications -> System Tools -> System Monitor

* Display real time process information
* Allows killing, re-nicing, sorting


### 4. Job Control

#### Background Process
* Append the ampersand to the command line: `firefox &`

#### Suspended Running Program

* Use `Ctrl+z
* Send STOP(19) signal

#### Manage Background Or Suspended Jobs
* List Job Numbers and Names:	`jobs`
* Resume in the Background:	`bg [%jobnum]`
* Resume in the Foreground:	`fg [%jobnum]`
* Send a Signal: `kill [SIGNAL] [%jobnum]`

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ping 127.0.0.1 &> /dev/null
^Z
[1]+  Stopped                 ping 127.0.0.1 &>/dev/null

[mitesh@Matrix ~]$ bg
[1]+ ping 127.0.0.1 &>/dev/null &

[mitesh@Matrix ~]$ firefox &
[2] 4162
{% endhighlight %}

**NOTE!:**	The number next to [2] after backgrounding firefox is the PID
{: .notice}

{% highlight bash %}
[mitesh@Matrix ~]$ jobs
[1]-  Running                 ping 127.0.0.1 &>/dev/null &
[2]+  Running                 firefox &
{% endhighlight %}

**NOTE!:**	The `+`` or `-`` signs next to the job numbers tells which job is the default <br>
`+` sign is the default job
{: .notice}
<br>

### 5. Scheduling Process

* One time jobs use `at`, Recurring jobs use `crontab`
<pre>
/-----------------------------------------------------------------------\
|									|
|	Create		|	at time		|	crontab -e	|
|	List		|	at -l		|	crontab -l	|
|	Details		|	at -c jobnum	|			|
|	Remove		|	at -d jobnum	|	crontab -r	|
|	Edit		|			|	crontab -e	|
|									|
\-----------------------------------------------------------------------/
</pre>
* Non-redirected output is mailed to the user
* The root can modify jobs for other users

#### at command

* Scheduling One Time Job with `at` command
* One Command Per Line
* Terminated With Ctrl+d


**Options**
<pre>
/-----------------------------------------------------------------------\
|	at 8:00am December 7			at 7 am Thursday	|
|	at midnight + 23 minutes		at now + 5 minutes	|
\-----------------------------------------------------------------------/
</pre>
<pre>
/-------------------------------------------------------------------------------------------------------\
|	Command		Alias		        Meanning					        |
|--------------------------------------------------------------------------------------------------------
|													|
|	atq	|	at -l		|	Lists the jobs currently pending.			|
|	atrm	|	at -d jobnum	|	Deletes the job.					|
|		|	at -c jobnum	|	Cats the full environment for the specified job.	|
|													|
\-------------------------------------------------------------------------------------------------------/
</pre>

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ at 0200
at> date
at> cal
at> <EOT>
job 1 at 2011-08-26 02:00


[mitesh@Matrix ~]$ atq
[mitesh@Matrix ~]$ at -l
1	2011-08-26 02:00 a mitesh
{% endhighlight %}

#### crontab command

* Scheduling Recurring Jobs with `crontab` command
* The cron mechanism is controlled by a process named `crond`.
* This process runs every minute and determines if an entry in user's cron tables need to be executed.

* The crontabs are stored in `/var/spool/cron/`
* The root can modify the jobs for other users with `crontab -u username` and any of the other options, such as `-e`.


**Crontab File Format**

* Comment lines begin with `#`.
* One entry per line, no limit to line length.
* Entry consist of five space-delimited fields followed by a command name.
* Fields are Minute, Hour, Day Of Month, Month, Day Of week.
* An asterisk (*) in a field represent all valid values.
* Multiple values are separated by commas.
* See `man 5 crontab` for more details

<pre>
/-----------------------------------------------------------------------\
|									|
|	Minute		|	0-59					|
|	Hour		|	0-23					|
|	Day Of Month	|	1-31					|
|	Month		|	1-12 (Or Jan, Feb, Mar, Etc)		|
|	Day Of Week	|	0-7  (Or Sun, Mon, Tue, Etc)		|
|				(0 or 7 = Sunday, 1 = Monday)		|
|									|
\-----------------------------------------------------------------------/
</pre>
<pre>
 # * * * * *  command to execute
 # │ │ │ │ │
 # │ │ │ │ │
 # │ │ │ │ └───── day of week (0 - 6) (0 to 6 are Sunday to Saturday, or use names; 7 is Sunday, the same as 0)
 # │ │ │ └────────── month (1 - 12)
 # │ │ └─────────────── day of month (1 - 31)
 # │ └──────────────────── hour (0 - 23)
 # └───────────────────────── min (0 - 59)
</pre>

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ crontab -e
#Min	Hour	DOM	Month	DOW	Command
0	0	31	10	*	mail -s "boo" $LOGNAME < boo.txt
0	2	*	*	*	netstat -tulpn | diff - /media/cdrom/baseline
0	4	*	*	1,3,5	find ~ -name core | xargs rm -f {}
{% endhighlight %}

##### crontab in details

* The `cron` mechanism is controlled by a process named `crond`.
* This process runs every minute and determines if an entry in user's cron tables need to be executed.

* The crontabs are stored in `/var/spool/cron/``
* The root can modify the jobs for other users with `crontab -u username` and any of the other options, such as `-e`.


##### Crontab File Format
* Comment lines begin with #.
* One entry per line, no limit to line length.
* Entry consist of five space-delimited fields followed by a command name.
* Fields are Minute, Hour, Day Of Month, Month, Day Of week.
* An asterisk (*) in a field represent all valid values.
* Multiple values are separated by commas.
* Special Time Specification Nicknames:
@reboot, @yearly, @annually, @monthly, @weekly, @daily, @hourly
* See man 5 crontab for more details

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ crontab -e
#Min    Hour    DOM     Month   DOW     Command
0       0       31      10      *       mail -s "boo" $LOGNAME < boo.txt
0       2       *       *       *       netstat -tulpn | diff - /media/cdrom/baseline
0       4       *       *       1,3,5   find ~ -name core | xargs rm -f {}

*/2	*	*	*	*	echo "Every 2 Minutes" &> /dev/tty1
*/5	*	*	*	*	echo "Every 5 Minutes" &> /dev/tty1
@reboot					echo "Runs Once After Reboot" &> /dev/tty1

[mitesh@Matrix ~]$ echo '*/15 8-17 * * 1-5 echo Breaktime' | crontab
{% endhighlight %}


##### The Cron Access Control
<pre>
/-----------------------------------------------------------------------------------------------------------------------------------------------\
|	---------------		--------------		Only root can install the crontab files.						|
|																		|
|	/etc/cron.allow		--------------		The root & All The Listed users in cron.allow can install the crontab files.		|
|																		|
|	---------------		/etc/cron.deny		All The users except The users in cron.deny can install the crontab files.		|
|																		|
|	/etc/cron.allow		/etc/cron.deny		The cron.deny file is ignored.								|
|							The root & All The Listed users in cron.allow can install the crontab files.		|
\-----------------------------------------------------------------------------------------------------------------------------------------------/
</pre>

**NOTE!:** Denying A User Through The Use Of Above Files Does Not Disable Their Installed crontab.
{: .notice}

#### System Crontab Files

* Different Format Than User Crontab Files
* Default System Crontab File Is `/etc/crontab`
* The `/etc/cron.d/` Directory Contains The Additional System Crontab Files

**Example:**
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

#run-parts
01	*	*	*	*	root	run-parts	/etc/cron.hourly
02	4	*	*	*	root	run-parts	/etc/cron.daily
02	4	*	*	0	root	run-parts	/etc/cron.weekly
42	4	1	*	*	root	run-parts	/etc/cron.monthly
{% endhighlight %}


**NOTE!:** The System Crontab Files Are Different From The Users Crontab Files<br>
In The System Crontab Files Sixth Field Is A Username, Which will Be Used To Execute The Commands.
<br><br>
The run-parts Is A Shell Script (/usr/bin/run-parts).<br>
The run-parts Shell Scripts Take One Argument - A Directory Name And Invokes All Of The Program In That Directory.
<br><br>
Thus, At 4:02 Every Morning, All Of The Executables In The /etc/cron.daily/ Directory Will Be Run As The root User.
{: .notice}

##### Default Daily Cron Jobs

* The `/etc/cron.daily` Are Usually Used For:

  * Clean Up Temporary Directories
  * Update mlocate & whatis Database
  * Perform Other Housekeeping Tasks

  ###### A) The tmpwatch:

  * Deletes All Files In /tmp Directory Which Is Not Accessed For 240 Hours (10 Days)
  * Deletes All Files In /var/tmp Directory Which Is Not Accessed For 720 Hours (30 Days)

  ###### B) The logrotate:

  * Keeps Log Files From Getting Too Large
  * Rotates Log Files On
  * Predefined Intervals (Weekly)
  * When Reach The Predefined Size
  * Old Files Are Optionally Compressed

  * Configuration Files:
    * `/etc/logrotate.conf`	(Global Configuration)
    * `/etc/logrotate.d/`	(Override Global Configuration)

**Example:**
The `/var/log/messages` Is Rotated Weekly To `/var/log/messages-yyyymmdd`



##### The Anacron System
* The Anacron Runs The Missed Cron Jobs When The System Boots.
* The Anacron Command Is Used To Run The Missed Daily,Weekly & Monthly Cron Jobs.

**Example:**

* According To The `/etc/crontab` File
* At 4:02 Every Morning, All Of The Executables In The /etc/cron.daily/ Directory Will Be Run As root User.
* Now Suppose Your Laptop Is Almost Always Off At The 4:02 AM, Then The mlocate & whatis Database Is Never Be Updated.

**Configuration File**

* `/etc/anacrontab`
* Field1:	If The Cron Jobs Not Been Run For The Specified No Of Days
* Field2:	Wait For The Specified No Of Minutes Before Runs
* Field3:	Job Identifier
* Field4:	The Cron Job To Run

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ cat /etc/anacrontab
# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# the maximal random delay added to the base delay of the jobs
RANDOM_DELAY=45
# the jobs will be started during the following hours only
START_HOURS_RANGE=3-22

# Period In Days	Delay In Minutes	Job-Identifier	Command
1		5			cron.daily	nice run-parts /etc/cron.daily
7		25			cron.weekly	nice run-parts /etc/cron.weekly
@monthly 	45			cron.monthly	nice run-parts /etc/cron.monthly
{% endhighlight %}

##### How Anacron Works

* According To The `/etc/crontab` File
* The 1st Command To Run Is 0anacron.
* The 0anacron Command Sets The Last Run Timestamp In A `/var/spool/anacron/cron.{daily,weekly,monthly}` Files.

* On The System Boot Up, The Anacron Commands Runs.
* The `/etc/anacrontab` File Specify How Often The Commands In `cron.daily/` `cron.weekly/` and `cron.monthly/` Should Be Runs.
* If These Commands Are Not Runs In This Time Then
* The Anacron Command Waits For The Specified No Of Minutes In The `/etc/anacrontab` File & Then Runs The Commands


### 6. Grouping Commands

* Two ways to group commands

#### Compound
* Example:  `date; who | wc -l`
* Commands run back to back

#### Subshell
* Commands inside parentheses are run in their own instance of bash, called subshell.
* Example:  `(date; who | wc -l)`
* All output is sent to a single STDOUT and STDERR


* Suppose you want to maintain a count of the number of users logged on,
Along with a time/date stamp, in a log file.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ date >> logfile
[mitesh@Matrix ~]$ who | wc -l >> logfile

[mitesh@Matrix ~]$ date; who | wc -l
Tue Aug 30 14:04:31 IST 2011
3

[mitesh@Matrix ~]$ date; who | wc -l >> logfile
Tue Aug 30 14:05:08 IST 2011

[mitesh@Matrix ~]$ (date; who | wc -l) >> logfile
{% endhighlight %}

### 7. Exit Status

* Processes report success or failure with an exit status.
* `0` for success
* `1-255` for failure

* `$?` stores the exit status of the most recent command
* `exit [num]` terminates and set status to num

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ping -c1 -w1 localhost &> /dev/null
[mitesh@Matrix ~]$ echo $?
0

[mitesh@Matrix ~]$ ping -c1 -w1 station999 &> /dev/null
[mitesh@Matrix ~]$ echo $?
2
{% endhighlight %}

### 8. Conditional Execution Operators

* `*`   Commands can be run conditionally based on exit status.
* `&&`	Represents conditional AND THEN
* `||`	Represents conditional OR ELSE

**NOTE!:**	When executing two commands separated by `&&`, <br>
The 2nd command runs if the 1st command exit successfully (Exit status 0). <br>
When executing two commands separated by `||`, <br>
The 2nd command runs if the 1st command fails (Exit status in the range of 1 to 255).
{:.notice}
<pre></pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ grep -q 'no_such_user' /etc/passwd || echo "No such user"
No such user

[mitesh@Matrix ~]$ ping -c1 -w2 localhost &> /dev/null \
> && echo "Localhost is up" \
> || (echo "Localhost is unreachable"; exit 1)
Localhost is up
[mitesh@Matrix ~]$ echo $?
0


[mitesh@Matrix ~]$ ping -c1 -w2 station999 &> /dev/null \
> && echo "Station999 is up" \
> || (echo "station999 is unreachable"; exit 1)
station999 is unreachable
[mitesh@Matrix ~]$ echo $?
1
{% endhighlight %}

{% highlight bash %}
#!/bin/bash
for x in $(seq 1 10)
do
  echo adding test$x
  (
    echo -ne "test$x\t"
    useradd test$x 2>&1 > /dev/null && mkpasswd test$x
  ) >> /tmp/userlog
done
echo 'cat /tmp/userlog to see new passwords'
{% endhighlight %}


### 9. test command
* The test command evaluates true or false scenarios to simplify conditional execution.
* Returns 0 for true
* Returns 1 for false

**NOTE!:**	Strings should be compared using a Mathematical Operator,
While Numbers are compared using an Abbreviation (-eq).
{: .notice}
<br><br>
**Examples:**
{% highlight bash %}
# Long Form
$ test "$A"  =  "$B" && echo "Strings are equal"
$ test "$A" -eq "$B" && echo "Integers are equal"

# Shorthand
$ [ "$A"  =  "$B" ] && echo "Strings are equal"
$ [ "$A" -eq "$B" ] && echo "Integers are equal"
{% endhighlight %}

#### File Tests
* Use the following command for complete list `man test`
<pre>
/-----------------------------------------------------------------------------------------------\
|												|
|	-d FILE		|	FILE exists and is a directory					|
|	-e FILE		|	FILE exists							|
|	-f FILE		|	FILE exists and is a regular file				|
|	-h FILE		|	FILE exists and is a symbolic link (same as -L)			|
|	-L FILE		|	FILE exists and is a symbolic link (same as -h)			|
|	-r FILE		|	FILE exists and read permission is granted			|
|	-s FILE		|	FILE exists and has a size greater than zero			|
|	-w FILE		|	FILE exists and write permission is granted			|
|	-x FILE		|	FILE exists and execute (or search) permission is granted	|
|	-O FILE		|	FILE exists and is owned by the effective user ID		|
|	-G FILE		|	FILE exists and is owned by the effective group ID		|
|												|
\-----------------------------------------------------------------------------------------------/
</pre>

**Example:**
{% highlight bash %}
$ [ -f ~/lib/functions ] && source ~/lib/functions
{% endhighlight %}

### 10. Scripting If Statements

* Every process reports an exit status.
* 0 for success
* 1-255 for failure

* Execute instructions based on a exit status of the command.
{% highlight bash %}
#!/bin/bash
if ping -c1 -w2 station1 &> /dev/null
then
  echo "Station1 is up"
elif grep "station1" ~/maintenance.txt &> /dev/null
then
  echo "Station1 is undergoing maintenance"
else
  echo "Station1 is unexpectedly DOWN!"
  exit 1
fi
{% endhighlight %}

* The exit status can be checked within the body of the if as shown in the example,
Or you can assign the exit status to a variable using a subshell, as in:

  * `STATUS=$(test -x /bin/ping6)`

* The if structure can be combined with conditional operator
{% highlight bash %}
#!/bin/bash
if test -x /bin/ping6; then
  ping6 -c1 ::1 &> /dev/null && echo "IPv6 stack is up"
elif test -x /bin/ping; then
  ping -c1 127.0.0.1 &> /dev/null && echo "No IPv6, but IPv4 stack is up"
else
  echo "Oops! This should not happen."
  exit 255
fi
{% endhighlight %}

**NOTE!:**	This script checks for the IPv6 version of the ping command (ping6) exists.<br>
If it does, it uses ping6 to send a test packet to the system's IPv6 Loopback Interface.<br>
Else The script checks for the IPv4 version of the ping command (ping) exists.<br>
If it does, it uses ping to send a test packet to the system's IPv4 Loopback Interface.<br>
If neither IPv6 nor IPv4 exists,<br>
Something is probably wrong and a non-zero return code is issued along with a warning message.<br>
{: .notice}

* FOR GOOD EXAMPLE OF REAL WORLD SCRIPTS, LOOK AT THE SCRIPTS IN `/etc/init.d/*`
