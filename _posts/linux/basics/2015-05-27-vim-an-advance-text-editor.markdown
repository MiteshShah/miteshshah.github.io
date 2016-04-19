---
layout: post
title: "VIM - an Advance Text Editor"
author:
modified: 2015-07-08T18:14:22+05:30
comments: true
categories: linux/basics
excerpt: "Newbie Guide - VIM An Advance Text Editor"
tags: [Linux, Linux Basics, Tutorials, VIM]
image:
  feature:
date: 2015-05-27T15:41:22+05:30
---

{% include _toc.html %}

### Introducing vim

* The newer version of `vi` text editor is called `vim` (Vi IMproved).
* `vim` is the standard Linux and UNIX text editor.
* The default text editor is `vi`, unless explicitly changed by the system administrator.

#### Advantages

* Speed:	Do more with fewer keystrokes
* Simplicity:	Not Dependence on Mouse/GUI
* Availability:	Included with most UNIX like OSes

#### Disadvantages

* Difficulty:	Difficult for beginners.
* Patience and persistence will lead to many great benefits on your way to becoming a Linux user.

#### Graphical vim

* `gvim` is the Graphical version of vim.
* Application -> Programming -> Vi IMproved
* Provided by vim-X11 package

### A Model Text Editor vim
* Keystroke behavior is dependent upon vim's mode.

#### Three Main Modes of vim

* `Command Mode` Default Mode of vim. Move cursor, cut/copy/paste text, change mode
* `Insert Mode`  Insert/Modify text
* `Ex Mode` Save, Save As, Quit, etc

* `Esc` Exit current mode
* `EscEsc` Always returns to command mode


### Basics of vim
* Open/Create file
* Modify file (Insert Mode)
* Save file	(Ex Mode)


#### Opening a file in vim

* `vim filename`
* If the file exists, the file is opened and the contents are displayed
* If the file does not exist, vim creates it when the edits are saved for the first time

**Examples:**
{% highlight bash %}
# Open a file named /etc/passwd in vim
[mitesh@Matrix ~]$ vim /etc/passwd

# Creates a file with vim
[mitesh@Matrix ~]$ vim /tmp/file
{% endhighlight %}

* This will create the file named /tmp/file, once you save it.

**NOTE!:** Until you save the file, all changes are kept in a temporary file. <br>
The temporary file, in this case, is /tmp/.file.swp <br>
(vim add a dot to the file name and adds the .swp extension). <br>
{: .notice }

* Once you save and quits the vim, it will create this file, write your changes to it and remove the temporary file.
* If the machine should crash while you are writing a file, this temporary file should still exist,
when the machine is working again, and it should contain your changes.


#### Insert Mode

Many commands will take you into insert mode.

* `i`:	Insert before the cursor
* `a`:	Append after the cursor

* `I`:	Insert at beginning of the line
* `A`:	Append at end of the line

* `o`:	Open a new line below the current line
* `O`:	Open a new line above the current line

**NOTE!:**	The letters i, a, I, A, o and O will not appears in your document,
but all other characters that you type will appears, until you exit insert mode.
To exit insert mode, hit the Esc key.
{: .notice}
<br>


#### Ex Mode

* Enter Ex Mode with `:`
* Creates a command prompt at bottom-left of the screen

* Common write/quit commands
<pre>
/-----------------------------------------------------------------------\
|	:q		|	quit					|
|	:q!		|	quits, even if changes are lost		|
|			|						|
|	:w		|	writes (saves) the file to disk		|
|	:wq		|	writes and quits			|
|			|						|
|	:w filename	|	save as funactionality			|
\-----------------------------------------------------------------------/
</pre>

#### Command Mode

* Default mode of vim.
* Keys describe Movements and Text manipulation Commands.
* Commands repeat when preceded by a number:

**Examples:**

* Right Arrow moves right one character
* 5+Right Arrow moves right five character


**Moving Around (Command Mode)**
<pre>
      ^
      k
< h       l >
      j
      v
</pre>
<pre>
/---------------------------------------------------------------\
|	Moves by characters	|	Arrow Keys		|
|				|	h    j    k  l		|
|				|	Left Down Up Right	|
|				|				|
|				|				|
|	Moves by words		|	w  b			|
|				|				|
|	Moves by sentence	|	(  )			|
|				|				|
|	Moves by paragraph	|	{  }			|
|				|				|
|	Jump to line x		|	xG  :x			|
|	Jump to end		|	G			|
\---------------------------------------------------------------/
</pre>

**Manipulating Text (Command Mode)**
<pre>
/-------------------------------------------------------------------------------\
|				|	Change	|	Delete	|	Yank	|
|				|	replace |	cut	|	copy	|
---------------------------------------------------------------------------------
|										|
|	Line			|	cc	|	dd	|	yy	|
|	Letter			|	cl	|	dl	|	yl	|
|	Word			|	cw	|	dw	|	yw	|
|										|
|	Sentence Above (Up)	|	c(	|	d(	|	y(	|
|	Sentence Below (Down)	|	c)	|	d)	|	y)	|
|										|
|	Paragraph Above	(Up)	|	c{	|	d{	|	y{	|
|	Paragraph Below	(Down)	|	c}	|	d}	|	y}	|
\-------------------------------------------------------------------------------/
</pre>

**Search and Replace (Command Mode)**

Search as in less

* `/`	`/dog` search downwards in the file for the string dog
* `?`	`?mouse` search upwards in the file for the string mouse
* `n/N` 	next/previous match

#### Search/Replace Operation

* `vi` and `vim` can perform search and replace operations, much like the `sed` command.
* The primary difference between the `sed` and `vim` is that -
  * Absent an Address, `sed` works on the entire file
  * Absent an Address, `vi` and `vim` works only on the current line, the line on which the cursor resides

* The default substitution delimiter is the `/` character.
* However, `vi` treats whatever character follows the `'s'` command as the delimiter.
{% highlight bash %}
:%s/\/dev\/hda/\/dev\/sda/gi
:%s'/dev/hda'/dev/sda'gi
{% endhighlight %}

**Address Range**

* Use `1,$`` or `%` for every line
* Use `x,y` for specific no of line

**Examples:**
{% highlight bash %}
:%s/cat/dog/gi
:1,$s/cat/dog/gi

:1,5s/cat/dog/gi
{% endhighlight %}



#### Put/Paste (Command Mode)
* Use p or P to put (paste) copied or deleted data.

**For Line Oriented Data (Line or Paragraph)**

* `p`	Puts the data below the current line
* `P`	Puts the data above the current line

**For Character Oriented Data (Letter Word Sentence)**

* `p`	Puts the data after the cursor
* `P`	Puts the data below the cursor


#### Undo (Command Mode)

* `u`	Undo most recent changes
* `U`	Undo all the changes to the current line since the cursor landed on the line

* `Ctrl+r`	Redo last 'undone' changes


#### Visual Mode

* It is possible to highlight characters, lines, or even block and then take actions on them (change, delete, yank etc)
* To begin visual mode, thus highlighting text, type:

* `v`	Start character oriented visual mode
* `V`	Start line oriented visual mode
* `Ctrl+v`	start block oriented visual mode

**NOTE!:** Visual mode is activated with Mouse in `gvim`. <br>
Use Arrow Keys or Shortcuts Keys of vim such as h j k l w b ( ) { } to highlight the text.
{: .notice}


### Using Multiple Windows

* The `vim` provides many features beyond those of `vi`.
* Multiple Documents can be viewed in a single vim screen.
<pre>
/---------------------------------------------------------------------------------------\
|											|
|	Ctrl+w n	|	Creates New Window With Empty Buffer (Horizontally)	|
|											|
|	Ctrl+w s	|	Split The Screen Horizontally	(Open Same File)	|
|	Ctrl+w v	|	Split The Screen Vertically	(Open Same File)	|
|											|
|	Ctrl+w +/-	|	Increse/Decrese Size Of The Window			|
|	Ctrl+w Arrow	|	Moves Between The Windows	(Works With h j k l)	|
|											|
\---------------------------------------------------------------------------------------/
</pre>

**NOTE!:** The standard open command (such as `:e` filename) can be used to change the file being edited in a window.
{: .notice }

* To start vim in windowing mode use `-o` options
{% highlight bash %}
vim -o .bashrc .bash_profile
{% endhighlight %}

* This will open both files Horizontally,
* `.bashrc` in the top of the window.
* `.bash_profile` in the bottom of the window.


### Configuring vi and vim

* Dozens of configuration items exist for `vi` and `vim`.
* To examine current configuration, run

* `:set`			List a small number of important configuration items.
* `:set all`		List all the configuration items
* `:help option-list`	Complete options lists



**Common Configuration Items**
{% highlight bash %}
:set number
:set showmatch
:set autoindent
:set ignorecase
:set wrapmargin=15
:set textwidth=65 (vim only)
{% endhighlight %}

**Show line Numbers `:set number`**
{% highlight bash %}
:set number		:set nu		:se nu
:set nonumber		:set nonu	:se nonu
{% endhighlight %}

**Highlight The Matching Brackets `:set showmatch`**
{% highlight bash %}
:set showmatch		:set sm		:se sm
:set noshowmatch	:set nosm	:se nosm
{% endhighlight %}

**Cause New Line To Inherit Independent Level Of The Previous Line `:set autoindent`**
{% highlight bash %}
:set autoindent		:set ai		:se ai
:set noautoindent	:set noai	:se noai
{% endhighlight %}

**Search Case-Insensitive `:set ignorecase`**
{% highlight bash %}
:set ignorecase		:set ic		:se ic
:set noignorecase	:set noic	:se noic
{% endhighlight %}

**Cause Text To Wrap When It Reaches 15 Characters From The Right Margin `:set wrapmargin=15`**
{% highlight bash %}
:set wrapmargin=15	:set wm=15	:se wm=15
:set wrapmargin=0	:set wm=0	:se wm=0
{% endhighlight %}

**Cause Text To Wrap When The Text Exceeds 65 Characters `:set textwidth=65`**
{% highlight bash %}
:set textwidth=65
:set textwidth=0
{% endhighlight %}

**NOTE!:** vim only
{: .notice}
<pre></pre>
**Create Folds As Per Defined Methods `:set foldmethod`**
{% highlight bash %}
# Methods
1. expr		3. manual	5. syntax
2. diff		4. marker	6. indent
{% endhighlight %}

**List Of Fold Commands `:help fold`**
{% highlight bash %}
:set foldmethod=indent	:set fdm=indent
:set foldmethod=syntax	:set fdm=syntax
{% endhighlight %}




**Configuring Permanently**

* Save all your changes in following file
{% highlight bash %}
~/.vimrc
~/.exrc
{% endhighlight %}



### Learning More

* Built-in vi/vim help
{% highlight bash %}
:help
:help topic
{% endhighlight %}

* `vimtutor` command is a great way to explore the functions of vim and get practice.

**NOTE!:** Use `:q` to exit help
{: .notice}
