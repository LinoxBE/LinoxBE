---
layout: post
title:  Backup DVD movie (Debian)
date:   2016-12-02 21:27:33 +0100
comments: true
categories: Linux Debian DVD Hack 
---
Take a copy of the DVD movie to your local disk:

{% highlight bash %}
vobcopy -m
{% endhighlight %}

Create an ISO file, ready to serve as backup of your DVD movie disk:

{% highlight bash %}
mkisofs -dvd-video -udf -o movie_backup.iso /movie_backup_directory/
{% endhighlight %}
