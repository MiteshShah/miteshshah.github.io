---
layout: post
title: "Kali Linux and Metasploit With Docker"
author:
modified:
comments: true
categories: mac
excerpt: "How to setup and run Kali Linux with Metasploit tool under the Mac OS X"
tags: [Mac, OSX, Kali, Linux, Docker]
image:
  url: https://www.kali.org/wp-content/uploads/2015/05/kali-linux-docker-images-798x284.png
  alt: Kali Linux and Metasploit With Docker
  title: Kali Linux and Metasploit With Docker
  feature:
date: 2015-07-31T11:31:02+05:30
---

{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

#### Install Required Softwares
{% highlight bash %}
$ brew cask install dockertoolbox
{% endhighlight %}

**NOTE!**: Make sure you have installed HomeBrew on your system,
If you don't have HomeBrew installed then <a href="/mac/things-to-do-after-installing-mac-os-x/#install-homebrew"> Click Here to Install HomeBrew </a>
{: .notice}

#### Create Virtual Machine
{% highlight bash %}
$ docker-machine create -d virtualbox kali
{% endhighlight %}

#### Export Environment Variables
{% highlight bash %}
$ eval "$(docker-machine env kali)"
{% endhighlight %}

#### Setting up a Kali Linux Docker Image
{% highlight bash %}
$ docker pull kalilinux/kali-linux-docker
$ docker run -t -i kalilinux/kali-linux-docker /bin/bash
{% endhighlight %}

#### Update Kali Linux & Install Metasploit & Other Kali Linux Tools
{% highlight bash %}
$ apt-get update && apt-get upgrade

# Install Metasploit Framework
$ apt-get install metasploit
{% endhighlight %}


### Save the Docker Container
* To commit(save) changes to your image and continue off where you left next time.
* First exit your image and get the container id.

#### Get Docker Container ID
{% highlight bash %}
$ docker ps -a
CONTAINER ID        IMAGE                         COMMAND             CREATED             STATUS                          PORTS               NAMES
da943d018ded        kalilinux/kali-linux-docker   "/bin/bash"         10 minutes ago      Exited (0) About a minute ago                       lonely_pas
{% endhighlight %}

#### Save Docker Container ID to An Image Name
{% highlight bash %}
# docker commit CONTAINER_ID YOUR_IMAGE_NAME
$ docker commit da943d018ded kali
b6655a1aa241cab87e339cdbc0fc7a81f2791a389e377b445ebd1004f181aff2
{% endhighlight %}

#### Start The Saved Docker CONTAINER
{% highlight bash %}
# docker run -t -i YOUR_IMAGE_NAME /bin/bash
$ docker run -t -i kali /bin/bash
{% endhighlight %}

#### Remove Docker Image
{% highlight bash %}
$ docker rm $(docker ps -aq)
{% endhighlight %}
