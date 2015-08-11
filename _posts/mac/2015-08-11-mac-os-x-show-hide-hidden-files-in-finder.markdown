---
layout: post
title: MAC OS X - Show/Hide Hidden Files in Finder
author:
modified:
comments: true
categories: mac
excerpt: "MAC OS X - Show/Hide Hidden Files in Finder"
tags: [MAC, OSX, Finder, Automater, ShellScript]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9196364/41ac3568-4048-11e5-8deb-a3e28939a1ab.png
  alt: MAC OS X - Show/Hide Hidden Files in Finder
  title: MAC OS X - Show/Hide Hidden Files in Finder
  feature:
date: 2015-08-11T16:30:05+05:30
---

* Launch Automator located in the applications.
* Select Service as the type of template to use for your new Automator task, and click the Choose button.
* In the Library pane, make sure Actions is selected, then underneath the Library item, click Utilities. This will filter the available workflow types to just those relating to utilities.
* In the filtered list of actions, click Run Shell Script and drag it to the workflow pane.
* At the top of the workflow pane are two drop-down menu items. Set the Service receives selected to `files or folders`. Set the in to `Finder`.
* Copy the entire shell script command that we created below, and use it to replace any text that may already be present in the Run Shell Script box.
<script src="https://gist-it.appspot.com/github/MiteshShah/admin/blob/master/Mac/ToggleHiddenFiles.sh"></script>
<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">
* From the Automator file menu, select Save, and then give the service a name.
* The name you select will appear as the menu item. I call mine Toggle Hidden Files.
* After saving quit the Atomater.
* And now, It's show time.

<img alt="MAC OS X - Show/Hide Hidden Files in Finder" src="https://cloud.githubusercontent.com/assets/1223371/9196772/27d452e8-404c-11e5-9e7a-1f4737a7affa.png">
