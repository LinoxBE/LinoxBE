---
layout: post
title:  Gluster on Debian Linux
date:   2016-12-02 21:29:22 +0100
comments: true
categories: Debian Linux Gluster
---
Install Gluster package on Debian

Download Gluster Debian package from [http://download.gluster.com/pub/gluster/glusterfs/LATEST/Debian/](http://download.gluster.com/pub/gluster/glusterfs/LATEST/Debian/).

To mount a Gluster Volume ("partition"), you need FUSE:

{% highlight bash %}
apt-get install fuse-utils
{% endhighlight %}

Setup Gluster daemon in /etc/glusterfs

{% highlight bash %}
server4:/etc/glusterfs# cat glusterd.vol
volume management
   type mgmt/glusterd
   option working-directory /etc/glusterd
   option transport-type socket
   option transport.socket.keepalive-time 10
   option transport.socket.keepalive-interval 2
end-volume

server4:/etc/glusterfs# cat glusterfsd.vol
volume posix
  type storage/posix
  option directory /data/glusterfs/export
end-volume

volume locks
  type features/locks
  subvolumes posix
end-volume

volume brick
  type performance/io-threads
  option thread-count 8
  subvolumes locks
end-volume

volume server
  type protocol/server
  option transport-type tcp/server
#  option auth.addr.brick.allow 192.168.0.104
  subvolumes brick
end-volume

server4:/etc/glusterfs# cat glusterfs.vol
volume server1
  type protocol/client
  option transport-type tcp/client
  option remote-host server1
  option remote-subvolume brick
end-volume

volume server2
  type protocol/client
  option transport-type tcp/client
  option remote-host server2
  option remote-subvolume brick
end-volume

volume server3
  type protocol/client
  option transport-type tcp/client
  option remote-host server3
  option remote-subvolume brick
end-volume

volume server4
  type protocol/client
  option transport-type tcp/client
  option remote-host server4
  option remote-subvolume brick
end-volume

volume distribute
  type cluster/distribute
  option min-free-disk 5%
  subvolumes server1 server2 server3 server4
end-volume

volume writebehind
  type performance/write-behind
  option cache-size 3MB
  option flush-behind on
  subvolumes distribute
end-volume

volume cache
  type performance/io-cache
  option cache-size 512MB
# option priority *.h:3,*.html:2,*:1
  subvolumes writebehind
end-volume

server4:/etc/glusterfs# /etc/init.d/glusterd restart
{% endhighlight %}

Create Volume on your servers (run from server4 here)

{% highlight bash %}
server4:/etc/glusterfs# gluster
gluster> peer probe server1
gluster> peer probe server2
gluster> peer probe server3
gluster> peer status
Number of Peers: 3

Hostname: server3
Uuid: 42d4b4e7-bf18-49ec-b693-eaed111b3ba1
State: Peer in Cluster (Connected)

Hostname: server2
Uuid: 37058b64-8439-403d-ab25-b4860a74c781
State: Peer in Cluster (Connected)

Hostname: server1
Uuid: d4d3662c-e9f1-4e38-861d-92671d26d81a
State: Peer in Cluster (Connected)
gluster> volumee create voltest server1:/voltest  server2:/voltest  server3:/voltest  server4:/voltest
Creation of volume voltest has been successful. Please start the volume to access data.
gluster> volume start voltest
Starting volume voltest has been successful
{% endhighlight %}

Mount Volume and access it

{% highlight bash %}
server4:/var/log/glusterfs# mount -t glusterfs server4:/voltest /mnt/glusterfs
server4:/var/log/glusterfs# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              30G  1.7G   27G   7% /
tmpfs                 2.0G     0  2.0G   0% /lib/init/rw
udev                   10M  564K  9.5M   6% /dev
tmpfs                 2.0G     0  2.0G   0% /dev/shm
/dev/sdb1             202G  153M  192G   1% /data
server4:/voltest
                      118G  6.8G  105G   7% /mnt/glusterfs
server4:/var/log/glusterfs# mount
/dev/sda1 on / type ext3 (rw,errors=remount-ro)
tmpfs on /lib/init/rw type tmpfs (rw,nosuid,mode=0755)
proc on /proc type proc (rw,noexec,nosuid,nodev)
sysfs on /sys type sysfs (rw,noexec,nosuid,nodev)
procbususb on /proc/bus/usb type usbfs (rw)
udev on /dev type tmpfs (rw,mode=0755)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
devpts on /dev/pts type devpts (rw,noexec,nosuid,gid=5,mode=620)
/dev/sdb1 on /data type ext2 (rw)
fusectl on /sys/fs/fuse/connections type fusectl (rw)
server4:/voltest on /mnt/glusterfs type fuse.glusterfs (rw,allow_other,default_permissions,max_read=131072)
{% endhighlight %}
