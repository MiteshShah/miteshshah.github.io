---
layout: post
title: How to Enable OpenSSL 1.0.2.a (TLSv1.1 & TLSv1.2) on CentOS 5 & RHEL5
author:
modified:
comments: true
categories: linux/centos
excerpt: "How to build OpenSSL & Curl Package to make connection on TLS1.2 Enable Websites."
tags: [Linux, CentOS, RHEL, OpenSSL, cURL]
image:
  url:
  alt: How to Enable OpenSSL 1.0.2.a (TLSv1.1 & TLSv1.2) on CentOS 5 & RHEL5
  title: How to Enable OpenSSL 1.0.2.a (TLSv1.1 & TLSv1.2) on CentOS 5 & RHEL5
  feature:
date: 2016-05-27T15:16:17+05:30
---


{% include _toc.html %}

### Issue

* RHEL5 & CentOS 5 Does not have TLSv1.2 support :(
* As lack of support we are not able to connect few websites which uses TLS1.2 SSL Protocol.

{% highlight bash %}
$ curl -Ivvv https://example.com
* Rebuilt URL to: https://example.com/
*   Trying 1.1.1.1...
* Connected to example.com (1.1.1.1) port 443 (#0)
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
* successfully set certificate verify locations:
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* TLSv1.0 (OUT), TLS handshake, Client hello (1):
* Unknown SSL protocol error in connection to example.com:443
* Closing connection 0
curl: (35) Unknown SSL protocol error in connection to example.com:443
{% endhighlight %}


### OpenSSL Package

This installs openSSL in `/usr/local/ssl` and will not overwrite the openSSL version already on disk so everything else compiled against the built in version of OpenSSL is still good to go.

{% highlight bash %}
$ mkdir ~/src/
$ cd ~/src/
$ wget https://www.openssl.org/source/openssl-1.0.2a.tar.gz
$ tar -zxvf openssl-*.tar.gz
$ cd openssl-*
$ ./config -fpic shared && make && make install
$ echo "/usr/local/ssl/lib" >> /etc/ld.so.conf
$ ldconfig
{% endhighlight %}

### cURL Package

This installs curl in `/usr/local/bin/curl`

{% highlight bash %}
$ cd /root/src
$ wget http://curl.haxx.se/download/curl-7.42.1.tar.gz
$ tar -xzvf curl-*.tar.gz
$ cd curl-*
$ ./configure --with-ssl=/usr/local/ssl --disable-ldap && make && make install
{% endhighlight %}


### Test Latest Curl
{% highlight bash %}
$ curl -Ivvv https://example.com
* Rebuilt URL to: https://example.com/
*   Trying 1.1.1.1...
* Connected to example.com (1.1.1.1) port 443 (#0)
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
* successfully set certificate verify locations:
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* TLSv1.2 (OUT), TLS Unknown, Unknown (22):
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Unknown (4):
* TLSv1.2 (IN), TLS change cipher, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using unknown / ECDHE-RSA-AES256-GCM-SHA384
* Server certificate:
*  subject: OU=Domain Control Validated; CN=*.example.com
*  start date: Nov 30 15:04:38 2015 GMT
*  expire date: Dec  5 15:54:06 2016 GMT
*  subjectAltName: host "example.com" matched cert's "*.example.com"
*  issuer: C=US; ST=Arizona; L=Scottsdale; O=GoDaddy.com, Inc.; OU=http://certs.godaddy.com/repository/; CN=Go Daddy Secure Certificate Authority - G2
*  SSL certificate verify ok.
> HEAD / HTTP/1.1
> Host: example.com
> User-Agent: curl/7.42.1
> Accept: */*
>
< HTTP/1.1 302 Found
HTTP/1.1 302 Found
< Server: nginx/1.8.1
Server: nginx/1.8.1
< Content-Type: text/html; charset=UTF-8
Content-Type: text/html; charset=UTF-8
< Connection: keep-alive
Connection: keep-alive
< Cache-Control: no-cache
Cache-Control: no-cache
< Date: Fri, 27 May 2016 09:55:35 GMT
Date: Fri, 27 May 2016 09:55:35 GMT
< Location: https://example.com/auth/login
Location: https://example.com/auth/login
< Set-Cookie: laravel_session=eyJpdiI6Im1udTFISFdNWmtaOGlcL0MrN0RHSVpnPT0iLCJ2YWx1ZSI6IlBpa3RrOWtWVVViMXQ2eTlibWlZcFZuTDk1R2RhQURtcmE2TjFrXC9cL3djVFwvc1lFelRpU2tEdlRDZXdRbjA1UHRNb3pMTTBCNHlpVTFlb3p2ZWpFUSt3PT0iLCJtYWMiOiJhMmI3MzBlNjJiZDc2NGE5MjcxNDZjZmI5YjA4YjMxOWQ4MjhjYTE5M2EyZTYwYThiM2Y1MGEwZmZhMjI0MzhhIn0%3D; expires=Fri, 27-May-2016 11:55:35 GMT; Max-Age=7200; path=/; httponly
Set-Cookie: laravel_session=eyJpdiI6Im1udTFISFdNWmtaOGlcL0MrN0RHSVpnPT0iLCJ2YWx1ZSI6IlBpa3RrOWtWVVViMXQ2eTlibWlZcFZuTDk1R2RhQURtcmE2TjFrXC9cL3djVFwvc1lFelRpU2tEdlRDZXdRbjA1UHRNb3pMTTBCNHlpVTFlb3p2ZWpFUSt3PT0iLCJtYWMiOiJhMmI3MzBlNjJiZDc2NGE5MjcxNDZjZmI5YjA4YjMxOWQ4MjhjYTE5M2EyZTYwYThiM2Y1MGEwZmZhMjI0MzhhIn0%3D; expires=Fri, 27-May-2016 11:55:35 GMT; Max-Age=7200; path=/; httponly

<
* Connection #0 to host example.com left intact
{% endhighlight %}

More Reference - <a href="http://forum.directadmin.com/showthread.php?t=51556">http://forum.directadmin.com/showthread.php?t=51556</a>
{: .notice }
