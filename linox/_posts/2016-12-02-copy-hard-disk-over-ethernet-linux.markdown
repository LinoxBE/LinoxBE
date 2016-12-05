---
layout: post
title:  Copy hard disk over Ethernet (Linux)
date:   2016-12-02 21:06:40 +0100
comments: true
categories: Linux
---
Sometimes you need to move a server from one server to another. And the only thing you can do, is copy the content of the disks. What I sometimes do, is stream the disk content with netcat.

ServerA sends the content of the disks to ServerB. Over Ethernet. With the tool netcat. Which is very fast, because it's using UDP.

I advice you to boot from a bootable Linux CD for streaming like that your hard disk (example: [Knoppix](http://www.knoppix.org))

{% highlight bash %}
ServerA:netcat ---> ServerB:netcat
{% endhighlight %}

To start that "migration", run:

{% highlight bash %}
root@ServerB# nc -p 2222 -l | dd of=/dev/sda
{% endhighlight %}
{% highlight bash %}
root@ServerA# dd if=/dev/hda | nc 10.32.107.140 2222
{% endhighlight %}

After that ServerB got all the data, both servers will stop their running script.
