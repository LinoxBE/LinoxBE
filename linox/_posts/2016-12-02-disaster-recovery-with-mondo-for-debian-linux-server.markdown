---
layout: post
title:  Disaster recovery (DR) with Mondo for Debian Linux server
date:   2016-12-02 21:34:17 +0100
comments: true
categories: Debian Linux
---
### Intro ###
If you want a disaster recovery for a Linux server (Debian here), you can find some very nasty tools on the web. The one I'm using for ages already, is Mondo (and Mindi).

For a Debian 5 Lenny server, go to [ftp://ftp.mondorescue.org/debian/](ftp://ftp.mondorescue.org/debian/) and download the packages mindi, mindi-busybox and mondo. Of course they have some dependencies you'll lto install together with your downloaded packages.

Once they are installed you can choose between 2 backup types:

### USB disk ###

By creating a boot-able USB disk, here with a 750GB USK disk:

{% highlight bash %}
mondoarchive -O -E "/mondo" -U -d /dev/sdc -s 745g -T "/mondo/tmp/"
{% endhighlight %}

Boot from that USB disk and you'll have your server again.

### CD/DVD ###

By creating boot-able ISO files, here 4GB DVD ISO files:

{% highlight bash %}
mondoarchive -iO -E "/mondo" -s 4g -S "/mondo/iso/" -T "/mondo/tmp/" -d "/mondo/"
{% endhighlight %}

Of course, folders /mondo, /mondo/iso and /mondo/tmp have to exist already.

In case you chose for the ISO option, you will find your boot-able ISO files inside /mondo.

Burn them on DVD:

{% highlight bash %}
cdrecord -v --dev=3,0,0 mondorescue-1.iso (run cdrecord -scanbus to identify your burner)
{% endhighlight %}

Boot from that DVD in case of a Disaster Recovery, and there you have your server again, just like it was when you took the mondoarchive!

### The end ###

Most important for Disaster Recovery, test it before you need it!!!
