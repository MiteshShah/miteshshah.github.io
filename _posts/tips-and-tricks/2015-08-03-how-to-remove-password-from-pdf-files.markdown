---
layout: post
title: "How to Remove Password From PDF Files"
author:
modified:
comments: true
categories: tips-and-tricks
excerpt: "How to Remove Password From PDF Files on Linux."
tags: [Linux, PDF, Password]
image:
  url:
  alt:
  title:
  feature:
date: 2015-08-03T12:36:51+05:30
---

{% include _toc.html %}

### Install PDFtk

#### Debian/Ubuntu Linux Distribution
{% highlight bash %}
$ sudo apt-get install pdftk
{% endhighlight %}

### Remove Password from PDF Files

{% highlight bash %}
$ pdftk secured.pdf input_pw mypassword output unsecured.pdf
{% endhighlight %}

**NOTE!**: You have to replace `mypassword` with your PDF file Password.
{: .notice}

* More Detailed Information for other PDF tasks <a href="https://www.pdflabs.com/docs/pdftk-cli-examples/">Check PDF LABS Examples</a>
