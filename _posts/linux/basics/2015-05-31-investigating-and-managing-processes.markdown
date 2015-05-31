---
layout: post
title: "Investigating and Managing Processes"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about process management/scheduling, signals and job control"
tags: []
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
