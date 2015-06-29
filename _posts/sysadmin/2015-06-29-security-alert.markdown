---
layout: post
title: "Security Alert - Get SMS Update For Wrong Password"
author:
modified:
comments: true
categories: sysadmin
excerpt: "As a Linux System Admin I'm always need to monitor system logs for any unauthorized activity like brute force attack or co-worker trying to guess the server passwords."
tags: [Linux, SysAdmin, SystemAdmin, Password, Twitter]
image:
  feature:
date: 2015-06-29T11:40:52+05:30
---

{% include _toc.html %}

* As a Linux System Admin I'm always need to monitor system logs for any unauthorized activity like brute force attack or co-worker trying to guess the server passwords.
* In this kind of situation system generate a special message in `/var/log/auth.log` file called `authentication failure`.


### Installation

#### Debian/Ubuntu Linux
{% highlight bash %}
[mitesh@shah ~]$ sudo apt-get install ruby-dev
{% endhighlight %}

#### Redhat/CentOS Linux
{% highlight bash %}
[mitesh@shah ~]$ sudo yum install ruby-devel
{% endhighlight %}

### Twitter Setup

#### Create New Twitter Account For Servers

* <a href="https://twitter.com/MiteshShah05">Personal Twitter Account</a>
* <a href="https://twitter.com/MiteshAlert">Serever Private Twitter Account</a>

* We need one Personal and one Private Twitter account.
* All the security alert messages posted on Private Twitter Account (MiteshAlert)
* I'm (MiteshShah05) the only follower of Private Twitter Account (MiteshAlert) so our security messages only display for me.

#### Install Twitter CommandLine Client t

* For More Detailed Information about Install and Configure t <a href="http://sferik.github.io/t/"> Click Here </a>
{% highlight bash %}
[mitesh@shah ~]$ gem install t
{% endhighlight %}

#### Configure t
{% highlight bash %}
[mitesh@shah ~]$ t authorised
Could not find command "authorised".
root@perk-cache:~# t authorize
Welcome! Before you can use t, you'll first need to register an
application with Twitter. Just follow the steps below:
  1. Sign in to the Twitter Application Management site and click
     "Create New App".
  2. Complete the required fields and submit the form.
     Note: Your application must have a unique name.
  3. Go to the Permissions tab of your application, and change the
     Access setting to "Read, Write and Access direct messages".
  4. Go to the API Keys tab to view the consumer key and secret,
     which you'll need to copy and paste below when prompted.

Press [Enter] to open the Twitter Developer site.

Open: https://apps.twitter.com
Enter your API key: 94g0557bTTNMQPSQf6DJYyrFG
Enter your API secret: j9H5dY0croAFiXJvmB2YjPZ32cawiqsqiCBeegOTtrTEy2bRhN

In a moment, you will be directed to the Twitter app authorization page.
Perform the following steps to complete the authorization process:
  1. Sign in to Twitter.
  2. Press "Authorize app".
  3. Copy and paste the supplied PIN below when prompted.

Press [Enter] to open the Twitter app authorization page.

Open: https://api.twitter.com/oauth/authorize?oauth_callback=oob&oauth_consumer_key=xxxxxxxxxx
Enter the supplied PIN: 1945192
Authorization successful.
{% endhighlight %}

### Security Alert

* You should need to create a crontab entry for the following shell script.
* So the following shell script runs every 10 minutes automatically.

<script src="https://gist.github.com/MiteshShah/3535ec562379c3c9d277.js"></script>

### Sample Tweet
<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">12:01:47 sshd[32239]: authentication failures; uid=0 tty=ssh ruser= rhost=X.XX.XX.XX</p>&mdash; Mitesh Shah (@MiteshAlert) <a href="https://twitter.com/MiteshAlert/status/615408754926858240">June 29, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Get SMS UpdateFor Above Tweets

* Open <a href="https://twitter.com/MiteshAlert">Serever Private Twitter Account</a> Page from your Personal Twitter Account
* Click on Settings <i class="fa fa-cog"></i>
* Click on Turn on mobile notifications.

<img src="https://cloud.githubusercontent.com/assets/1223371/8402334/6b4d1a2a-1e58-11e5-84f1-31bf468995ac.png">

* Feel free to comment below in case you face any problem.
