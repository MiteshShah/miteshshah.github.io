---
layout: post
title: "Guest Post Guidelines"
author: Hetal_Shah
comments: true
modified: 2015-05-22T16:04:52+05:30
categories: guide
excerpt: "Setup Jekyll and Write first guest post"
tags: [Guide, Jekyll, Guest Post]
image:
  feature:
date: 2015-05-18T17:49:06+05:30
---

{% include _toc.html %}

## Setup Work Directory
* We need to setup work directory and clone this blog.
{% highlight bash %}
mkdir -p ~/Github/
git clone git@github.com:MiteshShah/miteshshah.github.io.git
cd ~/Github/miteshshah.github.io
{% endhighlight %}

## Installation

* [Install Bundler](http://bundler.io) `sudo gem install bundler` and Run `bundle install` to install all dependencies (Jekyll, [Jekyll-Sitemap](https://github.com/jekyll/jekyll-sitemap), [Octopress](https://github.com/octopress/octopress), etc)


## Author Override
* Start by editing `authors.yml` file in the `_data` folder and add your authors using the following format.

{% highlight yaml %}

# Authors

Hetal_Shah:
  name: Hetal Shah
  web:
  email: hs8290hs@gmail.com
  bio: "Awesome Girl"
  gravatar: 7f1f44ee22b0131ca70b2b90bccb0c8b
  twitter: username
  google:
    plus: username

{% endhighlight %}

**Note!** You have to add your Gravatar Email Hash.
{: .notice}

## Create Post
* Create new post under guide categories
{% highlight bash %}
octopress new post "New Post Title" --dir guide
{% endhighlight %}

* Open atom and edit newly created post
{% highlight bash %}
atom atom _posts/guide/2015-05-18-new-post-title.markdown
{% endhighlight %}


* Assign author - Now add `author: Hetal_Shah` at the top of file.
* Enable comments - Now add `comments: true` at the top of file.
{% highlight yaml %}

---
layout: post
title: "Guest Post Guidelines"
author: Hetal_Shah
comments: true
modified:
categories: guide
excerpt: "How to setup jekyll and create guest post guidelines"
tags: [Guide, Jekyll, Guest Post]
image:
  feature:
date: 2015-05-18T17:49:06+05:30
---
{% endhighlight %}


* Check blog on web-browser at <a href="http://127.0.0.1:4000">http://127.0.0.1:4000</a>
{% highlight yaml %}

jekyll serve --config _config_dev.yml

{% endhighlight %}

* Now start writing your content and test it using above url, Once you are done with testing submit a pull request.
