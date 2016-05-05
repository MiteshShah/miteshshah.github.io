---
layout: post
title: "Perform Search and Replace on Webpages"
author:
modified:
comments: true
categories: tips-and-tricks
excerpt: "Sometimes we need functionality which can perform search and replace for us so we can directly copy paste codes."
tags: [Tips, Tricks, Chrome, Firefox, Safari]
image:
  feature:
date: 2015-07-01T14:01:39+05:30
---

* Sometimes we need functionality which can perform search and replace for us so we can directly copy paste codes.

**For Example:**
{% highlight text %}
[mitesh@shah ~]$ cat /etc/nginx/sites-available/example.com
server {
  server_name example.com www.example.com;

  access_log /var/log/nginx/example.com.access.log rt_cache;
  error_log /var/log/nginx/example.com.error.log;

  root /var/www/example.com/htdocs;
  index  index.html index.htm;

  location / {
    try_files $uri $uri/ /index.html;
  }

  include common/locations.conf;
  include /var/www/example.com/conf/nginx/*.conf;

}
{% endhighlight %}

* Instead of copy above codes and manually search and replace on vim we can directly perform search and replace on this page only.

### Perform Search & Replace

* Right click on this page
* Click on Inspect Element
* Click on Console
* Copy Paste Following Code and Press Enter.

{% highlight text %}
jQuery('body').html(function(index,html){
return html.replace(/example.com/g,'mitesh.com');
});
{% endhighlight %}
<img alt="Perform Search and Replace on Webpages" src="https://cloud.githubusercontent.com/assets/1223371/8450837/0e85a5b4-1ffc-11e5-859b-1f785c89f438.png">

* Once you press Enter check above code
* You can see example.com is replaced by mitesh.com just like below image.

<img alt="Perform Search and Replace on Webpages" src="https://cloud.githubusercontent.com/assets/1223371/8450834/08c3a5cc-1ffc-11e5-9894-27f12d30f343.png">
