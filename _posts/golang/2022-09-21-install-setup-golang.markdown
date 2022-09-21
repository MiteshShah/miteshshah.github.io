---
layout: post
title: Install/Setup & Run simple program in Golang
author:
modified:
comments: true
categories: golang
excerpt: Install/Setup & Run simple program in Golang
tags: [golang, Setup, Install, HomeBrew]
image:
  url:
  alt: Install/Setup & Run simple program in Golang
  title: Install/Setup & Run simple program in Golang
  feature:
date: 2022-09-21T14:06:37+05:30
---


{% include _toc.html %}

### Install & Setup Golang

{% highlight bash %}
$ brew install golang
$ go version
go version go1.19.1 darwin/amd64
{% endhighlight %}

#### hello.go
{% highlight golang %}
package main
import "fmt"
func main() {
	fmt.Println("Hello, 世界")
}
{% endhighlight %}

{% highlight bash %}
$ go run hello.go                       
Hello, 世界
{% endhighlight %}
