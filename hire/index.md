---
layout: page
title: Hire Me
tags: [MiteshShah, Hire, Hire Me, NGINX, PageSpeed, WordPress, Linux Expert]
modified: 2017-02-06T18:21:42.12345-05:30
comments:
blockads: true
image:
  feature: https://cloud.githubusercontent.com/assets/1223371/12032909/8342035e-ae43-11e5-9d9e-cb503820c4e9.png
  credit:
  creditlink:
---
### Mitesh Shah
NGINX/EasyEngine/WordPress Expert

<pre>
<input type="checkbox" name="checkbox" onclick="showMe('serversetup')" checked> Server Setup
<input type="checkbox" name="checkbox" onclick="showMe('websitemigration')" > Website Migration
<input type="checkbox" name="checkbox" onclick="showMe('fullymanagedhosting')" > Fully Managed Hosting
</pre>

<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script type="text/javascript">
$(':checkbox').on('change',function(){
 var th = $(this), name = th.prop('name');
 if(th.is(':checked')){
     $(':checkbox[name="'  + name + '"]').not($(this)).prop('checked',false);
  }
});

function showMe (box) {
    var vis = "none";
    if (box === "serversetup") {
      document.getElementById("serversetup").style.display = "block";
      document.getElementById("websitemigration").style.display = vis;
      document.getElementById("fullymanagedhosting").style.display = vis;
    }
    else if (box === "websitemigration") {
      document.getElementById("serversetup").style.display = vis;
      document.getElementById("websitemigration").style.display = "block";
      document.getElementById("fullymanagedhosting").style.display = vis;
    }
    else {
      document.getElementById("serversetup").style.display = vis;
      document.getElementById("websitemigration").style.display = vis;
      document.getElementById("fullymanagedhosting").style.display = "block";
    }
}
</script>

<div id="serversetup" style="display:block">
<pre>
Installing <b><a href="//ubuntu.com">Ubuntu</a></b> (Recommended) / <b><a href="//debian.org">Debian</a></b> Operating system on the server
Installing EasyEngine tool and setup fresh sites

Performance Tuning
* Database Tweaking

Security Hardening:
* Fail2ban setup
* Block all unused ports
* Free SSL (Let's Encrypt SSL)
* CalmAV Antivirus with Daily reporting
* Password-less login for extra security

Backup:
* Setup daily/weekly Backup on the same server or remote server

Price:
* Price starts from USD 200 only!

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_s-xclick">
<input type="hidden" name="hosted_button_id" value="64VAFJMLK2B3G">
<input type="image" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/btn/btn_paynowCC_LG.gif" border="0" name="submit" alt="PayPal – The safer, easier way to pay online!">
<img alt="" border="0" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/scr/pixel.gif" width="1" height="1">
</form>

</pre>
</div>

<div id="websitemigration" style="display:none">
<pre>
Installing <b><a href="//ubuntu.com">Ubuntu</a></b> (Recommended) / <b><a href="//debian.org">Debian</a></b> Operating system on the server
Installing EasyEngine tool and migrating sites from old server to new server

Performance Tuning
* Database Tweaking

Security Hardening:
* Fail2ban setup
* Block all unused ports
* Free SSL (Let's Encrypt SSL)
* CalmAV Antivirus with Daily reporting
* Password-less login for extra security

Backup:
* Setup daily/weekly Backup on the same server or remote server

Downtime:
* Zero downtime while migrating from the old server to the new server if old server has root user access

Price:
* Price starts from USD 350 only!

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_s-xclick">
<input type="hidden" name="hosted_button_id" value="HZMV9PDRWJXZN">
<input type="image" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/btn/btn_paynowCC_LG.gif" border="0" name="submit" alt="PayPal – The safer, easier way to pay online!">
<img alt="" border="0" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/scr/pixel.gif" width="1" height="1">
</form>

</pre>
</div>

<div id="fullymanagedhosting" style="display:none">
<pre>
Installing <b><a href="//ubuntu.com">Ubuntu</a></b> (Recommended) / <b><a href="//debian.org">Debian</a></b> Operating system on the server
Installing EasyEngine tool

Performance Tuning
* Database Tweaking

Security Hardening:
* Fail2ban setup
* Block all unused ports
* Free SSL (Let's Encrypt SSL)
* CalmAV Antivirus with Daily reporting
* Password-less login for extra security

Update Server:
* Monitor Server CPU/RAM/HDD
* Fix zeroday security issue on same day
* WordPress Theme/Plugin Updates weekly
* Install available Ubuntu/Debian package update weekly


Backup:
* Hourly, Daily, Weekly rotating backups
* Duplicity Backup for remote server or S3

Downtime:
* Zero downtime while migrating from the old server to the new server if old server has root user access

Price:
* Price starts from USD 250 only!

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_s-xclick">
<input type="hidden" name="hosted_button_id" value="B5LJ2A8PB9Q3Q">
<input type="image" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/btn/btn_subscribeCC_LG.gif" border="0" name="submit" alt="PayPal – The safer, easier way to pay online!">
<img alt="" border="0" alt="MiteshShah_HireMe" src="https://www.paypalobjects.com/en_GB/i/scr/pixel.gif" width="1" height="1">
</form>

</pre>
</div>

{% include _testimonials.html %}
