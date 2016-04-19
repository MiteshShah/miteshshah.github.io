---
layout: post
title: "The Linux Filesystem"
author:
modified:
comments: true
categories: linux/basics
excerpt: "Newbie Guide - All about Filesystem Partition, Management, HardLinks, Softlinks and Compress Data"
tags: [Linux, Linux Basics, Filesystem, Compress]
image:
  feature:
date: 2015-06-03T19:32:32+05:30
---

{% include _toc.html %}

### Partitions and Filesystems

* On Linux Systems, Disk Drives and Other Storage Media are Typically Divided into Partitions.
* Also Typically, A Partition is Formatted with a Filesystems.
* A Filesystem is a Data-Structure written to the Media that allows users to store and access the data.

#### Common Filesystems
<pre>
/-----------------------------------------------------------------------\
|	Filesystem	|	Used For				|
|-----------------------------------------------------------------------|
|									|
|	ISO 9660	|	CD					|
|	MSDOS		|	FloppyDisk				|
|									|
|	Ext2		|	FloppyDisk & Older Linux System		|
|	Ext3		|	Newer  Linux Filesystems		|
|	Ext4		|	Newest Linux Filesystems		|
|									|
|	GFS & GFS2	|	SAN					|
|									|
\-----------------------------------------------------------------------/
</pre>

### Inodes
* An Inode Table contains a list of all files in ext2, ext3 or ext4 filesystems.
* An Individual Entry in the Inode Table is called `inode` (index node).
* The `inode` is referenced by its inode number, which is unique within a Filesystem.
* The `inode` contains the metadata about the files (Everythings in Linux is a File) Such As:

1. The File Type
2. File Permissions
3. Link Count
4. User ID
5. Group ID
6. File Size
7. Time Stamps
8. Location Of The Data On The HardDisk
9. Other Metadata About The Files

**NOTE!:** Link Count - The Number Of Filenames Associated With The Inode Number.
{: .notice}
<br>

#### Directories
* We Commonly Think of a Directory as a Container, For The Files and Other Directories.
* In Fact, The Directory is a Mapping Between The Filenames and Inode Numbers.
* Filenames used by Humans to Reference a File
* Inode Numbers used by Computers to Reference a File

* When the filename is referenced by the command or application, Linux referenced the directory in which the file resides,
To Determines the inode number associated with the filename.

#### Check inode numbers
{% highlight bash %}
[mitesh@Matrix ~]$ ls -li
total 36
786448 drwxr-xr-x. 2 mitesh mitesh 4096 Sep  9 11:46 Desktop
786452 drwxr-xr-x. 2 mitesh mitesh 4096 Aug  4 22:08 Documents
786449 drwxr-xr-x. 2 mitesh mitesh 4096 Aug  4 22:08 Downloads
786453 drwxr-xr-x. 2 mitesh mitesh 4096 Aug  4 22:08 Music
786629 -rw-rw-r--. 1 mitesh mitesh    0 Sep  9 11:47 MyData
786454 drwxr-xr-x. 3 mitesh mitesh 4096 Sep  3 10:52 Pictures
786451 drwxr-xr-x. 2 mitesh mitesh 4096 Aug  4 22:08 Public
786632 drwxrwxr-x. 2 mitesh mitesh 4096 Sep  9 10:45 Studynotes
786450 drwxr-xr-x. 2 mitesh mitesh 4096 Aug  4 22:08 Templates
786455 drwxr-xr-x. 3 mitesh mitesh 4096 Aug 20 16:51 Videos
{% endhighlight %}

**NOTE!:** The `inode` number for the `MyData` file is 786629
{: .notice}
<br>

#### cp command and inodes
* Allocates a free `inode` number, Placing a new entry in the Inode Table.
* Creates a new Directory Entry, Associating a name with the `inode` number.
* Copies Data in to the New File.

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -li penguin
786864 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 12:16 penguin

[mitesh@Matrix ~]$ cp penguin tux

[mitesh@Matrix ~]$ ls -li penguin tux
786864 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 12:16 penguin
786879 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 12:16 tux
{% endhighlight %}

#### mv command and inodes
* IF THE DESTINATION OF THE MV COMMAND IS ON THE SAME FILESYSTEM AS THE SOURCE
  * Creates New Directory Entry, With The New Filename.
  * Deletes Old Directory Entry, With The Old Filename.

**NOTE!:** Has No Impact On The Inode Table Except For The Timestamp Or The Location Of The Data NO DATA IS MOVED!
{: .notice}
<pre></pre>

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -li tux
786879 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 12:16 tux

[mitesh@Matrix ~]$ mv tux fedora

[mitesh@Matrix ~]$ ls -li fedora
786879 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 12:16 fedora
{% endhighlight %}

**NOTE!:** When We Move The Files On The Same Filesystem Then The File's Inode Number & Data On The Hard Disk Does Not Move. <br>
What Moves Is The Entry In The Directory Only
{: .notice}

* IF THE DESTINATION OF THE MV COMMAND IS ON THE DIFFERENT FILESYSTEM:
  * THE MV COMMAND ACT AS A COPY AND REMOVE



#### rm command and inodes
* Removes the Directory Entry
* Place the Data Blocks on the Free List.
* Decrements the link count - Made inode number available for reuse.

**NOTE!:** DATA IS NOT ACTUALLY REMOVED, BUT WILL BE OVERWRITTEN WHEN THE DATA BLOCKS ARE REUSED BY SOME OTHER FILES
{: .notice}


### Hard Links
* The Hard Link is a path name that references an `inode` number,
All the files are hard linked at least once.

* A Hard Links adds an additional Directory Entry to reference a single file.
* One Physical File On The System
* Each Directory Entry Reference The Same Inode Number
* Increment The Link Count
* The `rm` command decrement the link count
* File exists as long as one link remains
* When the link count is zero, The file is removed

**A Hard Link Restrictions**

* Can Not Span Across The Filesystems, Partitions or Drives.
They share the `inode` number and `inode` table which is unique on the filesystem

* It Is Not Possible To Use `ln` Command To Create Additional Hard Links To Directories.

**Syntax:**
{% highlight bash %}
ln filename linkname
{% endhighlight %}

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ls -li fedora
786579 -rw-rw-r--. 1 mitesh mitesh 0 Sep  9 13:57 fedora

[mitesh@Matrix ~]$ ln fedora redhat

[mitesh@Matrix ~]$ ls -li fedora redhat
786579 -rw-rw-r--. 2 mitesh mitesh 0 Sep  9 13:57 fedora
786579 -rw-rw-r--. 2 mitesh mitesh 0 Sep  9 13:57 redhat
{% endhighlight %}

**The Most Notable Effect Here Is**

* The `fedora` & `redhat` two files pointing to the same `inode` number.
* The `fedora` & `redhat` both are regular files, An additional hard link is not a separate file type.
* The link count has been incremented to 2 because two pathname point to the same file.


### Symbolic(Soft) Links
* The Symbolic(Soft) Link is a file that points to another file.
* Most actions such as cat or less will be performed on the underlying file except the `rm` command.

* The `ls -l` command display the link name and referenced file.
{% highlight bash %}
lrwxrwxrwx. 1 mitesh mitesh   11 Sep  9 14:22 passwd -> /etc/passwd
{% endhighlight %}

* File Type: `l` for symmbolic(soft) link
* The Size Of The Symbolic Link = Number Of Characters In The Pathname (11 Characters in `/etc/passwd`)

**Syntax:**
{% highlight bash %}
ln -s filename linkname
{% endhighlight %}

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ ln -s /etc/passwd passwd

[mitesh@Matrix ~]$ ls -li /etc/passwd passwd
3016841 -rw-r--r--. 1 root    root    1829 Sep  8 11:56 /etc/passwd
786579 lrwxrwxrwx. 1 mitesh mitesh   11 Sep  9 14:25 passwd -> /etc/passwd
{% endhighlight %}

**The Most Notable Effect Here Is**

* A Symbolic Links has its own inode number
That means symbolic link is a separate file from the original file.
* The Symbolic Links Permissions are odd and irrelevant
Which is symbolic link control access rights.


### The Seven Fundamental Filetypes Of Linux & Unix
<pre>
/-----------------------------------------------\
|	-	|	Regular File		|
|	d	|	Directory		|
|	l	|	Symbolic Link		|
|	b	|	Block Special File	|
|	c	|	Character Special File	|
|	p	|	Named Pipe		|
|	s	|	Socket			|
\-----------------------------------------------/
</pre>

#### Block Special File
* Block Special File is used to communicate with hardware a block of data at a time
512 Bytes, 1024 Bytes, 2048 Bytes: Whatever is appropriate for that type of hardware.

#### Character Special File
* Character Special File is used to communicate with hardware one character at a time.
* Generally Block & Character Special Files are located in the `/dev` Directory.
* Run the following command to see the listing:
{% highlight bash %}
[mitesh@Matrix ~]$ ls -l /dev | less
drwxr-xr-x. 5 root root         100 Sep  9 13:42 disk
lrwxrwxrwx. 1 root root           3 Sep  9 13:42 dvd1 -> sr0
brw-rw----. 1 root disk    253,   0 Sep  9 13:42 dm-0
crw-rw----. 1 root root     10,  56 Sep  9 13:42 autofs
...output omitted...
{% endhighlight %}

#### Named Pipe
* Named Pipe is a file that passes data between the processes.
* Named Pipe  does not store any data itself but passes data between
One process writing data into a named pipe &
Another process reading data from the named pipe
* Named Pipe can be created using the `mkfifo` command.

#### Socket
* The Socket File is a stylized mechanism for inter-process communications.
* It is extremely rare for a user or even a system administrator to explicitly create a socket.




### Checking Free Space

#### df command
* Reports Disk Space Usages On Per Filesystem Basis.
* It Reports Total Disk Space, Used Space, Available Space, Used % and Mounted on Informations.

**Options:**
{% highlight bash %}
-h:	Human Readable
{% endhighlight %}

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ df
Filesystem			1K-blocks	Used		Available	Use%	Mounted on
/dev/mapper/vg_Matrix-lv_root	51606140	2478524		46506176	6%	/
tmpfs				  506024	    420		  505604	1%	/dev/shm
/dev/sda1			  495844	  30610		  439634	7%	/boot
/dev/mapper/vg_Matrix-lv_home	61265956	3301428 	54852388	6%	/home


[mitesh@Matrix ~]$ df -h
Filesystem			Size	Used	Avail	Use%	Mounted on
/dev/mapper/vg_Matrix-lv_root	 50G	2.4G	 45G	6%	/
tmpfs				495M	420K	494M	1%	/dev/shm
/dev/sda1			485M	 30M	430M	7%	/boot
/dev/mapper/vg_Matrix-lv_home	 59G	3.2G	 53G	6%	/home


[mitesh@Matrix ~]$ df -h /opt
Filesystem			Size	Used	Avail	Use%	Mounted on
/dev/mapper/vg_Matrix-lv_root	 50G	2.4G	 45G	6%	/
{% endhighlight %}

#### du command
* Reports Disk Space Usages On Given Directory & All Subdirectories.

**Options:**
{% highlight bash %}
-h:	Human Readable
-s:	Display Only Total Used Space By Given Directory
{% endhighlight %}

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ du -s /etc
22184	/etc
{% endhighlight %}

#### baobab
* The baobab is a Graphical Disk Usage Analyzer.
* Applications -> System Tools -> Disk Usage Analyzer
* Reports Disk Space Uses Graphically.




### Removable Media
* Before Accessing The Removable Media, The Filesystem's on the Removable Media Must Be Mounted.
* In GNOME & KDE, Removable Media is usually Auto-Mounted in `/media/label`.
* Where label is Disk Label or Device ID.

* In console root manually mounts devices

**Examples:**
{% highlight bash %}
[root@Matrix ~]# mkdir /mnt/cdrom
[root@Matrix ~]# mount /dev/cdrom /mnt/cdrom

# After Finished Your Work
[root@Matrix ~]# umount /dev/cdrom
{% endhighlight %}

#### Floppy Disk
* The `mtools` is a utilities to access DOS Disks in Linux & Unix.
* Mtools  is  a  collection  of  tools to allow Linux & Unix systems to manipulate MS-DOS files:
read, write, and move around files on an MS-DOS Filesystem (Typically a floppy disk).

**Examples:**
{% highlight bash %}
[mitesh@Matrix ~]$ mdir a:
...output omitted...

[mitesh@Matrix ~]$ mcopy file.txt a:
{% endhighlight %}

#### CDs & DVDs
* Automatically Mounted In GNOME/KDE
* Accessible From:
  * CD-ROM Desktop Icon
  * Computer Desktop Icon -> CD/DVD Drive
  * `/media/cdrom` OR `/media/disk_label`

**Ejected With**

* eject
* eject `/dev/cdrom`
* Right Click -> Eject


#### USB Media
* Detected By The Kernel As SCSI Devices.
`/dev/sda`, `/dev/sdaX`, `/dev/sdb`, `/dev/sdbX`, etc.
* Automatically Mounted In GNOME/KDE
* Accessible From:
  * Removable Disk Desktop Icon
  * Computer Desktop Icon -> Removable Disk
  * `/media/disk` OR `/media/disk_label`

**Ejected With**

* umount `/dev/sdaX`
* umount `/dev/sdbX`
* Right Click -> Safely Remove Device


### Archiving Files & Compressing Archives
* Tape Archive (`tar`) is used to creates Archives.
* The Tar Saves Many Files Together into a
Single Disk Archive & Restore Individual Files from the Archive.
* Easier to Backup, Store and Transfer The Files.

* The Tar Also Support The Following Compression Algorithms:
  * gzip		- gunzip	Extension of .tar.gz OR .tgz
  * bzip2		- bunzip2	Extension of .tar.bz2


#### Creating Listing & Extracting File Archives

**Action Argument**

* `-c`:	Create	An Archive
* `-t`:	List	An Archive
* `-x`:	Extract	An Archive

**Typically Required**

* `-f`: 	Use Archive File

**Optional Argument**

* `-v`:	Verbose
* `-z`:	Use gzip  Compression
* `-j`:	Use bzip2 Compression


**Examples:**
{% highlight bash %}
# Create an Archive of the /etc Directory
[root@Matrix ~]# tar -cvf /tmp/etc.tar /etc
...output omitted...

# List the contents of the tar Archive
[root@Matrix ~]# tar -tf /tmp/etc.tar | less

# Extract the contents of the Archive
[root@Matrix ~]# mkdir /tmp/NewETC
[root@Matrix ~]# cd /tmp/NewETC
[root@Matrix ~]# tar -xvf /tmp/etc.tar
...output omitted...
{% endhighlight %}

**Gzip or Gunzip Compression**
{% highlight bash %}
[root@Matrix ~]# tar -zcvf /tmp/etc.tar.gz /etc
...output omitted...

[root@Matrix ~]# tar -tvf /tmp/etc.tar.gz | less

[root@Matrix ~]# mkdir /tmp/NewETC
[root@Matrix ~]# cd /tmp/NewETC
[root@Matrix ~]# tar -zxvf /tmp/etc.tar.gz
...output omitted...
{% endhighlight %}

**Bzip2 or Bunzip2 Compression**
{% highlight bash %}
[root@Matrix ~]# tar -jcvf /tmp/etc.tar.bz2 /etc
...output omitted...

[root@Matrix ~]# tar -tvf /tmp/etc.tar.bz2 | less

[root@Matrix ~]# mkdir /tmp/NewETC
[root@Matrix ~]# cd /tmp/NewETC
[root@Matrix ~]# tar -jxvf /tmp/etc.tar.bz2
...output omitted...
{% endhighlight %}



#### Creating Extracting File Archives - Other Tools

##### Zip & Unzip
* Support pkzip comapatible archives.

**Examples:**
{% highlight bash %}
[root@Matrix ~]# zip -r etc.zip /etc
[root@Matrix ~]# unzip etc.zip
{% endhighlight %}

##### Graphical Archive Manager:
* Applications -> Accessories -> Archive Manager
* The file-roller is a Multi Format Graphical Archive Manager.
