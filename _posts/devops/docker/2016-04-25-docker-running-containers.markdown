---
layout: post
title: Docker - Running Containers
author:
modified:
comments: true
categories: devops/docker
excerpt: "Running container through pre-existing docker images"
tags: [Devops, Docker]
image:
  url:
  alt: Docker - Running Containers
  title: Docker - Running Containers
  feature:
date: 2016-04-25T12:05:46+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Prerequisites
For running docker containers you must have docker engine installed. Please refer <a href="/devops/docker/docker-installation/"> Docker Installation </a> section for Installation instructions.

### Runnig containers
For running applications inside containers you will need a commad `docker run`. Following format will help you understand running docker containers.

{% highlight bash %}
$ docker run <image> <command>
{% endhighlight %}

To Illustrate above command lets launch first container.

{% highlight bash %}
$ docker run ubuntu /bin/echo Hello world
Hello world
{% endhighlight %}

In above example
*ubuntu* - Is an image you run containers with. you are running command within Ubuntu OS. This image if not present locally is pulled from public image registry **Docker Hub**

*/bin/echo* - Is the command you run inside container running with specified image.

### Running Interactive containers

In previous section containers remained active for the time of execution of command specified. So Let's run the containers interactive manner.

{% highlight bash %}
$ docker run -t -i <image> <command>
{% endhighlight %}

To Illustrate above command lets run another container.

{% highlight bash %}
$ docker run -t -i ubuntu /bin/bash
root@af8bae53bdd3:/#
{% endhighlight %}

In this example
*-t* - Is the flag which assigns psuedo-tty or terminal inside container.
*-i* - Is the flag which allows you to run container interactively.
*/bin/bash* - Launches a bash shell inside container.

Lets run some more commands inside the same container

{% highlight bash %}
root@af8bae53bdd3:/# pwd
/
root@af8bae53bdd3:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
{% endhighlight %}

When completed, run the exit command or enter Ctrl-D to exit the interactive shell. This will also stop the running container.

### Running Daemonized containers
If you want to run your application in daemonized way, you can do this in following manner

{% highlight bash %}
$ docker run -d <image> <command>
{% endhighlight %}

To Illustrate above command lets run a container

{% highlight bash %}
docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147
{% endhighlight %}

In above example
*-d* - flag runs the container in the background (to daemonize it).

In this post we have studied three different ways to run containers.
