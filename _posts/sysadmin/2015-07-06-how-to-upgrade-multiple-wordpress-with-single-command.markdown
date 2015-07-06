---
layout: post
title: "How to Upgrade Multiple WordPress With Single Command"
author:
modified:
comments: true
categories: sysadmin
excerpt: "How to Upgrade Multiple WordPress, WordPress Plugins and WordPress Themes with single command."
tags: [Linux, SysAdmin, SystemAdmin, WP-CLI, WordPress, ShellScript]
image:
  feature:
date: 2015-07-06T17:13:53+05:30
---

{% include _toc.html %}

### Requirements

* PHP 5.3.2 or later
* WordPress 3.5.2 or later

### Shell Script To Upgrade Multiple WordPress

**WARNING!**: Before Execute following script take BACKUP<br>
The following script is tested on Single WordPress Only.
{: .notice}

* The following shell script asked
  * Web Root Path [/var/www]
  * Web Root Prefix [htdocs]
  * Web Root Owner Name [www-data]
  * Web Root Group Name [www-data]

* After that It' sfirst check wp-cli is installed or not.

{% gist MiteshShah/cd4d86cbdfa60a069aa9 %}


#### Before Website status
<img src="https://cloud.githubusercontent.com/assets/1223371/8521910/6aa51ab0-2406-11e5-94d9-7327640b3625.png">

#### Execute Shell Script to Upgrade WordPress
{% highlight bash %}
$ wget https://gist.githubusercontent.com/MiteshShah/cd4d86cbdfa60a069aa9/raw/831d8d41f0411b97682649a57c119e137619a986/wp_update.sh
$ sudo bash wp_update.sh
Enter Web Root Path [/var/www]:
Enter Web Root Path [htdocs]:
Enter Web Root Path [www-data]:
Enter Web Root Path [www-data]:

WEB_ROOT = /var/www
WEB_ROOT_PREFIX = htdocs
WEB_ROOT_OWNER = www-data
WEB_ROOT_GROUP = www-data

PHP binary:	/usr/bin/php5
PHP version:	5.6.10-1+deb.sury.org~trusty+1
php.ini used:	/etc/php5/cli/php.ini
WP-CLI root dir:	phar://wp-cli.phar
WP-CLI global config:
WP-CLI project config:
WP-CLI version:	0.19.2

Found wp-admin at /var/www/example.com/htdocs/wp-admin
Updating to version 4.2.2 (en_US)...
Downloading update from https://downloads.wordpress.org/release/wordpress-4.2.2-new-bundled.zip...
Unpacking the update...
Success: WordPress updated successfully.

Downloading update from https://downloads.wordpress.org/plugin/akismet.3.1.2.zip...
Unpacking the update...
Installing the latest version...
Removing the old version of the plugin...
Plugin updated successfully.
Success: Translations updates are not needed for the 'English (US)' locale.
Success: Updated 1/1 plugins.
name	old_version	new_version	status
akismet	3.0.2	3.1.2	Updated

Enabling Maintenance mode...
Downloading update from https://downloads.wordpress.org/theme/twentyfourteen.1.4.zip...
Unpacking the update...
Installing the latest version...
Removing the old version of the theme...
Theme updated successfully.
Downloading update from https://downloads.wordpress.org/theme/twentythirteen.1.5.zip...
Unpacking the update...
Installing the latest version...
Removing the old version of the theme...
Theme updated successfully.
Downloading update from https://downloads.wordpress.org/theme/twentytwelve.1.7.zip...
Unpacking the update...
Installing the latest version...
Removing the old version of the theme...
Theme updated successfully.
Disabling Maintenance mode...

Success: Translations updates are not needed for the 'English (US)' locale.
Success: Updated 3/3 themes.
{% endhighlight %}

#### After Website Status

<img src="https://cloud.githubusercontent.com/assets/1223371/8521950/c091d882-2406-11e5-91e3-6ab171df781e.png">
