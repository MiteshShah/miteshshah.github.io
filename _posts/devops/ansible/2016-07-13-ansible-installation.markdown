---
layout: post
title: Ansible Installation
author:
modified:
comments: true
categories: devops/ansible
excerpt: "Ansible is an IT automation tool. It can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates."
tags: [DevOps, Ansible]
image:
  url:
  alt: Ansible Installation
  title: Ansible Installation
  feature:
date: 2016-07-13T17:14:35+05:30
---


{% include _toc.html %}

### Prerequisites

* Python 2.6/2.7

### Installation

#### Install Ansible on Ubuntu

{% highlight bash %}
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
{% endhighlight %}

#### Install Ansible on CentOS

{% highlight bash %}
$ sudo yum install epel-release
$ sudo yum install ansible
{% endhighlight %}

#### Install Ansible on Mac OS X

{% highlight bash %}
$ brew install ansible
{% endhighlight %}

**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}
