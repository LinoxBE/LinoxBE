---
layout: post
title:  Mount Iscsi Devices (RedHat)
date:   2016-12-02 21:22:19 +0100
comments: true
categories: Linux RedHat iSCSI 
---

Install iSCSI initiator utilities:

{% highlight bash %}
yum install iscsi-initiator-utils
{% endhighlight %}

Find iSCSI targets:

{% highlight bash %}
iscsiadm --mode discovery --type sendtargets --portal 10.0.0.10
{% endhighlight %}

You maybe need to restart iSCSI and change iSCSI settings:

{% highlight bash %}
/etc/init.d/iscsi stop
/etc/init.d/iscsi start
vi /etc/iscsi/initiatorname.iscsi
vi /etc/iscsi/iscsid.conf
{% endhighlight %}

You can label the partitions on the iSCSI, easier for automatic mounts after reboots:

{% highlight bash %}
e2label /dev/sdc1 /data1
e2label /dev/sdd1 /data2
{% endhighlight %}

Mount the iSCSI disk:

{% highlight bash %}
mount LABEL=/data1 /data1
{% endhighlight %}
