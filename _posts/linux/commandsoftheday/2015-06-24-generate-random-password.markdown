---
layout: post
title: "Generate Random Password"
author:
modified:
comments: true
categories: linux/commandsoftheday
excerpt: "How to generate some random password by using single command"
tags: [Linux, CommandLine, CommandOfTheDay, Password]
image:
  feature:
date: 2015-06-24T20:04:42+05:30
---

{% include _toc.html %}

#### Generate AlphaNumeric Random Password
{% highlight bash %}
[mitesh@Matrix ~]$ cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 15 | head -n10
k8eViiHzPvquY18
5niBD7oMDNwr9aF
cdqas4okZ9L9jLx
0C8X3s2g6I1RmKc
6Gafnp9ttaRmlHX
FrgSa94hNwoEibR
zipj2iWfA3ct368
g6BrhR3edlxEPeW
vjZ9zlde3ZXdslh
sn2BbDk4HEW101q
{% endhighlight %}

#### Generate AlphaNumeric & Symbols Random Password
{% highlight bash %}
[mitesh@Matrix ~]$ cat /dev/urandom | tr -dc 'a-zA-Z0-9!@#$%^&()'| fold -w 15 | head -n10
(QacABq(&kHXWge
v3OX@Ku@CH4A)yB
DdS#LW^)bzXZc$#
zony@vzx@UMj5s0
myxs#h7w(pn2lrF
wjky8a79KigEVm1
V8NMMkuV)^!I17#
VNuQ0m^6ksejNY6
Cvkd(Teawofvzk7
qN7sZVOI0kwwZ#K
{% endhighlight %}

#### Generate Random Password Using openssl command
{% highlight bash %}
[mitesh@Matrix ~]$ openssl rand -hex 64 | fold -w 15  
95aeaedcc6b5214
d0af42129cba416
c21b28bdf34fc46
b97302c5255114e
68baeecb1fbc750
130bd41520805ba
8371ba327971627
a9c51eab84050b0
97a61595
{% endhighlight %}

#### Generate Random Password Using pwgen command
{% highlight text %}
# Let's install pwgen command
mitesh@Matrix ~]$ sudo apt-get install pwgen

# Let's Generate Some Random Passwords
mitesh@Matrix ~]$ pwgen -cny
Kae)Z2uv ahQu2Zu= It@oh3Iz lo'uni4H ahH:eeJ1 Wath/aP2 moo4Jee$ ooN[ee4x
iesha\V4 ahCha_P8 joa3El)u Oaj4na.k ahb4Ep(i zoo+Ba2u Yuc?ei4o ooz8oaY%
Pho7aw|u Eiha"e0o hae>K4og Rus4xai( Eu+la0Vi EiD`eit2 Eiwe#th1 Gai+w'e3
SheB7ai< Xah#To8l hie9waP_ Gah+g4ju cho<es6A Hoh,gh3e ooL8bah^ iCie`s7a
{% endhighlight %}
