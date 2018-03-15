---
layout: post
title:  Software Raid On Mac Mini
date:   2016-12-02 21:52:50 +0100
comments: true
categories: Linux Debian Mac 
---
I have an older Mac Mini, with a PowerPC (PPC) CPU. On that Mac Mini I installed Debian PPC. Not difficult. But because I think the internal disk is dying, and because I have an older spare USB disk, I want to combine both to something more reliable for my data. Using software raid, buitl in into the Linux kernel.

Here is my command history.

Check what's on the internal disk and create the partition we will use for the software raid:

{% highlight bash %}
mini:~# fdisk /dev/hda
Command (? for help): p
/dev/hda
        #                    type name                  length   base      ( size )  system
/dev/hda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/hda2         Apple_Bootstrap untitled                3907 @ 64        (  1.9M)  NewWorld bootblock
/dev/hda3         Apple_UNIX_SVR2 untitled            58593751 @ 3971      ( 27.9G)  Linux native
/dev/hda4              Apple_Free Extra               97703766 @ 58597722  ( 46.6G)  Free space

Block size=512, Number of Blocks=156301488
DeviceType=0x0, DeviceId=0x0


Command (? for help): c
First block: 58597722
Length (in blocks, kB (k), MB (M) or GB (G)): 97703766
Name of partition: raid
Command (? for help): p
/dev/hda
        #                    type name                  length   base      ( size )  system
/dev/hda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/hda2         Apple_Bootstrap untitled                3907 @ 64        (  1.9M)  NewWorld bootblock
/dev/hda3         Apple_UNIX_SVR2 untitled            58593751 @ 3971      ( 27.9G)  Linux native
/dev/hda4         Apple_UNIX_SVR2 raid                97703766 @ 58597722  ( 46.

Block size=512, Number of Blocks=156301488
DeviceType=0x0, DeviceId=0x0


Command (? for help): w
IMPORTANT: You are about to write a changed partition map to disk.
For any partition you changed the start or size of, writing out
the map causes all data on that partition to be LOST FOREVER.
Make sure you have a backup of any data on such partitions you
want to keep before answering 'yes' to the question below!

Write partition map? [n/y]: y
The partition map has been saved successfully!

Syncing disks.

Partition map written to disk. If any partitions on this disk
were still in use by the system (see messages above), you will need
to reboot in order to utilize the new partition map.

Command (? for help): q
Load the software raid modules into the kernel
mini:~# modprobe md
mini:~# modprobe raid1
mini:~# ll /dev/md*
total 0
Create the partition for raid on the external USB disk:
mini:~# fdisk /dev/sda
/dev/sda
Command (? for help): p
No partition map exists
Command (? for help): ?
Notes:
  Base and length fields are blocks, which are 512 bytes long.
  The name of a partition is descriptive text.

Commands are:
  h    help
  p    print the partition table
  P    (print ordered by base address)
  i    initialize partition map
  s    change size of partition map
  b    create new 800K bootstrap partition
  c    create new Linux partition
  C    (create with type also specified)
  d    delete a partition
  r    reorder partition entry in map
  w    write the partition table
  q    quit editing (don't save changes)
Command (? for help): i
size of 'device' is 320173056 blocks:
new size of 'device' is 320173056 blocks
Command (? for help): p
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2              Apple_Free Extra              320172992 @ 64        (152.7G)  Free space

Block size=512, Number of Blocks=320173056
DeviceType=0x0, DeviceId=0x0

Command (? for help): c
First block: 64
Length (in blocks, kB (k), MB (M) or GB (G)): 97703766
Name of partition: raid
Command (? for help): p
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2         Apple_UNIX_SVR2 raid                97703766 @ 64        ( 46.6G)  Linux native
/dev/sda3              Apple_Free Extra              222469226 @ 97703830  (106.1G)  Free space

Block size=512, Number of Blocks=320173056
DeviceType=0x0, DeviceId=0x0

Command (? for help): c
First block: 97703830
Length (in blocks, kB (k), MB (M) or GB (G)): 222469226
Name of partition: p
Command (? for help): p
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2         Apple_UNIX_SVR2 raid                97703766 @ 64        ( 46.6G)  Linux native
/dev/sda3         Apple_UNIX_SVR2 p                  222469226 @ 97703830  (106.1G)  Linux native

Block size=512, Number of Blocks=320173056
DeviceType=0x0, DeviceId=0x0

Command (? for help): p
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2         Apple_UNIX_SVR2 raid                97703766 @ 64        ( 46.6G)  Linux native
/dev/sda3         Apple_UNIX_SVR2 p                  222469226 @ 97703830  (106.1G)  Linux native

Block size=512, Number of Blocks=320173056
DeviceType=0x0, DeviceId=0x0

Command (? for help): ?
Notes:
  Base and length fields are blocks, which are 512 bytes long.
  The name of a partition is descriptive text.

Commands are:
  h    help
  p    print the partition table
  P    (print ordered by base address)
  i    initialize partition map
  s    change size of partition map
  b    create new 800K bootstrap partition
  c    create new Linux partition
  C    (create with type also specified)
  d    delete a partition
  r    reorder partition entry in map
  w    write the partition table
  q    quit editing (don't save changes)
Command (? for help): w
IMPORTANT: You are about to write a changed partition map to disk.
For any partition you changed the start or size of, writing out
the map causes all data on that partition to be LOST FOREVER.
Make sure you have a backup of any data on such partitions you
want to keep before answering 'yes' to the question below!

Write partition map? [n/y]: y
The partition map has been saved successfully!

Syncing disks.

Partition map written to disk. If any partitions on this disk
were still in use by the system (see messages above), you will need
to reboot in order to utilize the new partition map.

Command (? for help): q
Check the partitions on the disks and create the software raid device:
mini:~# fdisk -l /dev/sda
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2         Apple_UNIX_SVR2 raid                97703766 @ 64        ( 46.6G)  Linux native
/dev/sda3         Apple_UNIX_SVR2 p                  222469226 @ 97703830  (106.1G)  Linux native

Block size=512, Number of Blocks=320173056
DeviceType=0x0, DeviceId=0x0

mini:~# fdisk -l /dev/hda
/dev/hda
        #                    type name                  length   base      ( size )  system
/dev/hda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/hda2         Apple_Bootstrap untitled                3907 @ 64        (  1.9M)  NewWorld bootblock
/dev/hda3         Apple_UNIX_SVR2 untitled            58593751 @ 3971      ( 27.9G)  Linux native
/dev/hda4         Apple_UNIX_SVR2 raid                97703766 @ 58597722  ( 46.6G)  Linux native

Block size=512, Number of Blocks=156301488
DeviceType=0x0, DeviceId=0x0

mini:~#  mdadm --create /dev/md0 -l1 -n2 /dev/hda4 /dev/sda2
mdadm: array /dev/md0 started.
mini:~# ll /dev/md*
brw-rw---- 1 root disk 9, 0 2010-02-19 21:16 /dev/md0

/dev/md:
total 0

mini:~# dmesg |tail -30
[   19.432691] eth0: Link is up at 100 Mbps, full-duplex.
[   19.438743] eth0: Pause is disabled
[   22.311163] NET: Registered protocol family 10
[   22.312745] lo: Disabled Privacy Extensions
[   22.844675] RPC: Registered udp transport module.
[   22.845625] RPC: Registered tcp transport module.
[   22.922990] Installing knfsd (copyright (C) 1996 okir@monad.swb.de).
[   23.023021] NFSD: Using /var/lib/nfs/v4recovery as the NFSv4 state recovery directory
[   23.037864] NFSD: starting 90-second grace period
[   27.476122] NET: Registered protocol family 5
[   33.160475] eth0: no IPv6 routers present
[90914.899398] md: raid1 personality registered for level 1
[91322.284112] sd 1:0:0:0: [sda] 320173056 512-byte hardware sectors (163929 MB)
[91322.285965] sd 1:0:0:0: [sda] Write Protect is off
[91322.286829] sd 1:0:0:0: [sda] Mode Sense: 27 00 00 00
[91322.286833] sd 1:0:0:0: [sda] Assuming drive cache: write through
[91322.287719]  sda: [mac] sda1 sda2 sda3
[91324.309578] sd 1:0:0:0: [sda] 320173056 512-byte hardware sectors (163929 MB)
[91324.311429] sd 1:0:0:0: [sda] Write Protect is off
[91324.312287] sd 1:0:0:0: [sda] Mode Sense: 27 00 00 00
[91324.312292] sd 1:0:0:0: [sda] Assuming drive cache: write through
[91324.313155]  sda: [mac] sda1 sda2 sda3
[91430.835341] md: bind<hda4>
[91430.852070] md: bind<sda2>
[91430.852910] md: md0: raid array is not clean -- starting background reconstruction
[91431.010544] raid1: raid set md0 active with 2 out of 2 mirrors
[91431.033542] md: resync of RAID array md0
[91431.034418] md: minimum _guaranteed_  speed: 1000 KB/sec/disk.
[91431.035331] md: using maximum available idle IO bandwidth (but not more than 200000 KB/sec) for resync.
[91431.036353] md: using 128k window, over a total of 48851776 blocks.
See here the creation of your partition:
mini:~# cat /proc/mdstat
Personalities : [raid1]
md0 : active raid1 sda2[1] hda4[0]
      48851776 blocks [2/2] [UU]
      [>....................]  resync =  2.2% (1112640/48851776) finish=38.3min speed=20727K/sec

unused devices: <none>
mini:~# cat /proc/mdstat
Personalities : [raid1]
md0 : active raid1 sda2[1] hda4[0]
      48851776 blocks [2/2] [UU]
      [===>.................]  resync = 15.8% (7722368/48851776) finish=33.1min speed=20682K/sec

unused devices: <none>
Create a filesystem (ext3) on your software raid partition and mount it:
mini:~#  mkfs.ext3 /dev/md0
mke2fs 1.41.3 (12-Oct-2008)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
3055616 inodes, 12212944 blocks
610647 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=0
373 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 26 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
mini:~# mkdir /data
mini:~# mount /dev/md0 /data/
mini:~# vi /etc/fstab

mini:~# cat /proc/mdstat
Personalities : [raid1]
md0 : active raid1 sda2[1] hda4[0]
      48851776 blocks [2/2] [UU]
      [=====>...............]  resync = 26.9% (13163840/48851776) finish=31.7min speed=18709K/sec

unused devices: <none>
{% endhighlight %}
