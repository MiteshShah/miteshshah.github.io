---
layout: post
title: How to Find Out AWS S3 Bucket Human Readable Size
author:
modified:
comments: true
categories: devops/AWS/S3
excerpt: "s3cmd is the best command-line tool to manage S3 buckets. Sometimes, you will need to know the size of some of your bucket"
tags: [DevOps, AWS, S3]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/16793780/20cc9308-48f1-11e6-8c9f-242070064a8f.png
  alt: How to Find Out AWS S3 Bucket Human Readable Size
  title: How to Find Out AWS S3 Bucket Human Readable Size
  feature:
date: 2016-07-13T11:49:40+05:30
---


{% include _toc.html %}

<img src="{{ page.image.url }}" alt="{{ page.image.alt }}" title="{{ page.image.title }}">

### Install & Configure S3CMD

{% highlight bash %}
# Download S3CMD
$ git clone https://github.com/s3tools/s3cmd.git
# Configure S3CMD
$ cd s3cmd
$ ./s3cmd --configure
{% endhighlight %}


### Find Out S3 Bucket Size
{% highlight bash %}
$ s3cmd du s3://my-bucket
5885516953 s3://my-bucket

# Human Readable
$ s3cmd du -H s3://my-bucket
5G   s3://my-bucket

# Find All S3 Bucket Human Readable Size
$ s3cmd du -H s3://
18G      144 objects s3://bucket1/
19G      7441 objects s3://bucket2/
143G     10333 objects s3://bucket3/
{% endhighlight %}
