---
layout: post
title: How to Create a Bootable Ubuntu USB
author:
modified:
comments: true
categories: linux/ubuntu
excerpt: "Create Bootable Ubuntu USB & Fix gfxboot.c32 not a com32r image boot problem"
tags: [Linux, Ubuntu, Bootable]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/10477714/2b0eb6c2-7276-11e5-8a3d-9913a459ba81.jpg
  alt: Bootable Ubuntu USB
  title: Bootable Ubuntu USB
  feature:
date: 2015-10-14T13:17:17+05:30
---
{% include _toc.html %}

### Requirements

* At least 2GB USB Flash Drive
* Download <a href="http://www.ubuntu.com/download">Ubuntu ISO Image</a>

### Create Bootable Ubuntu USB

* Open the dash and search for **Startup Disk Creator**.
* Select the **Startup Disk Creator** to launch the app.
<img alt="Bootable Ubuntu USB" src="https://cloud.githubusercontent.com/assets/1223371/10477915/927dad62-7277-11e5-8157-48d34d3df7e2.jpg" alt="Startup Disk Creator" title="Startup Disk Creator">
* Click **Other** to choose the downloaded ISO file.
* Select the file and click **Open**.
<img alt="Bootable Ubuntu USB" src="https://cloud.githubusercontent.com/assets/1223371/10477992/1b4ed7ba-7278-11e5-8b62-bb6af2e48e36.jpg">
* Select the USB stick in the bottom box and click **Make Startup Disk**.
<img alt="Bootable Ubuntu USB" src="https://cloud.githubusercontent.com/assets/1223371/10477993/1bff7890-7278-11e5-9078-cc2b1ed2d378.jpg">
* That’s it! When the process completes, you’ll be ready to restart your computer and begin installing Ubuntu.

### Fix gfxboot.c32 not a com32r image

<img alt="Bootable Ubuntu USB" src="https://cloud.githubusercontent.com/assets/1223371/10478112/d742907e-7278-11e5-96c1-8a2b036e64ad.png">

* Hit `TAB`
* Then you will see a set of options (live, live install, etc).
* Now type `live` and press `ENTRER`.

### Reference

* <a href="http://askubuntu.com/questions/486602/ubuntu-14-04-lts-live-usb-boot-error-gfxboot-c32not-a-valid-com32r-image"> Ask Ubuntu</a>
* <a href="https://bugs.launchpad.net/ubuntu/+source/usb-creator/+bug/1325801">Ubuntu Bug<a>
