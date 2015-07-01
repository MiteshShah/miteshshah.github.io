---
layout: post
title: "Transform Any Browser Window Into Text Editor"
author:
modified:
comments: true
categories: tips-and-tricks
excerpt: "Transform Google Chrome, Firefox or Safari Web Browser Windows Into Quick Edit Notepad."
tags: [Tips, Tricks, Chrome, Firefox, Safari]
image:
  feature:
date: 2015-07-01T13:34:35+05:30
---

{% include _toc.html %}

* Sometimes we need to write down one or two lines notes.
* In that case we can use our web browser as a quick edit notepad.
* All you need to do is type the following code into the browser's URL.
* It will make your page as editable just like notepad.

### Simple Text Editor
{% highlight text %}
data:text/html, <html contenteditable>
{% endhighlight %}

<img src="https://cloud.githubusercontent.com/assets/1223371/8450218/39db076e-1ff6-11e5-906a-0e3bcb8d3cfd.png">

### Sublime Type Text Editor
{% highlight text %}
data:text/html,<title>Editor</title><style type="text/css">#e{font-size: 16px; position:absolute;top:0;right:0;bottom:0;left:0;}</style><div id="e"></div><script src="http://d1n0x3qji82z53.cloudfront.net/src-min-noconflict/ace.js" type="text/javascript" charset="utf-8"></script><script>var e=ace.edit("e");e.setTheme("ace/theme/monokai");e.getSession().setMode("ace/mode/javascript");</script>
{% endhighlight %}
<img src="https://cloud.githubusercontent.com/assets/1223371/8450357/cdfef5b2-1ff7-11e5-9d27-84d779efe177.png">

**NOTE!** For quick access save above code as a bookmark into your web browser.
{: .notice}
