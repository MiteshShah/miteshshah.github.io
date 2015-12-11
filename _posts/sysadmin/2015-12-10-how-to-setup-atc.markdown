---
layout: post
title: How to Setup Augmented Traffic Control (ATC)
author:
modified:
comments: true
categories: sysadmin
excerpt: "Augmented Traffic Control (ATC) is a tool to simulate network conditions. It allows controlling the connection that a device has to the internet. Developers can use ATC to test their application across varying network conditions, easily emulating high speed, mobile, and even severely impaired networks."
tags: [sysadmin]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/11738228/be286120-a006-11e5-8611-7e09d5ae19c0.png
  alt: How to Setup Augmented Traffic Control (ATC)
  title: How to Setup Augmented Traffic Control (ATC)
  feature:
date: 2015-12-10T15:38:46+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

#### Installation

{% highlight bash %}
# Install Virtual Environment
$ sudo pip install virtualenv
$ mkdir -p ~/dev/atc
$ virtualenv ~/dev/atc/venv
$ source ~/dev/atc/venv/bin/activate

# Package installation
$ pip install atc_thrift atcd django-atc-api django-atc-demo-ui django-atc-profile-storage

# Configuration
$ cd ~/dev/atc
$ django-admin startproject atcui
$ cd atcui/

{% endhighlight %}

* At this stage, we have a brand new django project that needs to be configured.
* First, let's enable our django apps by editing `atcui/settings.py` and adding the following apps to `INSTALLED_APPS`:

{% highlight bash %}
$ vim atcui/settings.py
INSTALLED_APPS = (
    ...
    # Django ATC API
    'rest_framework',
    'atc_api',
    # Django ATC Demo UI
    'bootstrap_themes',
    'django_static_jquery',
    'atc_demo_ui',
    # Django ATC Profile Storage
    'atc_profile_storage',
)
{% endhighlight %}

* Now, we need to route requests to the right django app.
* This is done by editing `atcui/urls.py` and adding the routes to `urlpatterns`:

{% highlight bash %}
$ vim atcui/urls.py
...
from django.conf.urls import include
from django.views.generic.base import RedirectView

urlpatterns = [
    ...
    # Django ATC API
    url(r'^api/v1/', include('atc_api.urls')),
    # Django ATC Demo UI
    url(r'^atc_demo_ui/', include('atc_demo_ui.urls')),
    # Django ATC profile storage
    url(r'^api/v1/profiles/', include('atc_profile_storage.urls')),
    url(r'^$', RedirectView.as_view(url='/atc_demo_ui/', permanent=False)),
]
{% endhighlight %}

* Finally, migrate the django DB and run the server locally

{% highlight bash %}
$ python manage.py migrate
{% endhighlight %}

* That is it, the UI should be set up. Now let's run it!


#### Running it

* Now that we have configured the UI, let's run the `atcd` service and run the UI.
* You need to make sure that you are using your virtual environment.

##### Starting atcd service

{% highlight bash %}
$ test -z $VIRTUAL_ENV && source ~/dev/atc/venv/bin/activate
$ sudo atcd
{% endhighlight %}

##### Start atc demo ui

{% highlight bash %}
$ test -z $VIRTUAL_ENV && source ~/dev/atc/venv/bin/activate
$ cd ~/dev/atc/atcui
$ python manage.py runserver 0.0.0.0:8000
{% endhighlight %}

* You should now be able to access the UI at the following address
<a href="http://localhost:8000">http://localhost:8000</a> and get the following page.

<img  alt="atc_demo_ui" src="https://cloud.githubusercontent.com/assets/1223371/11738418/96451c0a-a008-11e5-89dc-a23903cac5a9.png">

**NOTE**! If you are using Django 1.9 then might me you face stylesheet issue. <br>
Refer: <a href="https://github.com/facebook/augmented-traffic-control/issues/216">Dirty Fix</a>
{: .notice }

#### Import Network Profiles

{% highlight bash %}
$ cd ~/dev/atc/
$ git clone https://github.com/facebook/augmented-traffic-control.git
$ cd augmented-traffic-control/utils/
$  bash restore-profiles.sh localhost:8000
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/2G-DevelopingRural.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/2G-DevelopingUrban.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/3G-Average.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/3G-Good.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/Cable.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/DSL.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/Edge-Average.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/Edge-Good.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/Edge-Lossy.json
Added profile ~/dev/atc/augmented-traffic-control/utils/profiles/NoConnectivity.json
{% endhighlight %}

#### Test Augmented Traffic Control

* Open <a href="http://localhost:8000">http://localhost:8000</a>
* Click on `Authentication` and add your device ip address and access token, click on `Authorize` button
* Click on `Profiles` and select your testing network profile.
* Now at top of page you will find `Update Shaping` button click on it.

<img alt="Test Augmented Traffic Control (ATC)" src="https://cloud.githubusercontent.com/assets/1223371/11739381/0358fa20-a011-11e5-8a49-d80dd2034f86.png">
