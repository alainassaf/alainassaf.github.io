---
layout: post
title: Creating an ISO Partition on DOM0
subtitle: XenServer
date: 2011-11-18
tags: [Administration, LabInABox, Other Blogs, XenServer]
---
<strong>Intro</strong>

I recently updated my <a href="/2010-07-08-its-my-lab-in-a-box/" target="_blank"><b>lab machine</b></a> to XenServer 6.0 and I wanted to create a local ISO repository on the DOM0 partition. I have 3 physical drives, one 250GB drive that holds the host partition and its backup and two 500GB drives that host VM’s. I know that only 8GB on the 250 GB drive are used for the host and its backup, so I wanted to create the local ISO repository in the remaining space.

<strong>Stop! Linux Time</strong>

Connect to the CLI of your XenServer.

fdisk –l shows my current partition tables

```
[root@MARLINSPIKE ~]# fdisk -l

WARNING: GPT (GUID Partition Table) detected on '/dev/sda'! The util fdisk doesn't support GPT. Use GNU Parted.

Disk /dev/sda: 500.1 GB, 500107862016 bytes
255 heads, 63 sectors/track, 60801 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

Disk /dev/sda doesn't contain a valid partition table

WARNING: GPT (GUID Partition Table) detected on '/dev/sdb'! The util fdisk doesn't support GPT. Use GNU Parted.

Disk /dev/sdb: 500.1 GB, 500107862016 bytes
255 heads, 63 sectors/track, 60801 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

Disk /dev/sdb doesn't contain a valid partition table

WARNING: GPT (GUID Partition Table) detected on '/dev/sdc'! The util fdisk doesn't support GPT. Use GNU Parted.

Disk /dev/sdc: 250.0 GB, 250059350016 bytes
256 heads, 63 sectors/track, 30282 cylinders
Units = cylinders of 16128 * 512 = 8257536 bytes

Device Boot      Start         End      Blocks   Id  System
/dev/sdc1   *           1       30283   244198583+  ee  EFI GPT
```

Dom0 contains 3 partitions. The first is where the XenServer host resides. The second is the host backup. The final partition is the rest of the unused space on the 250GB drive. In my file system, this is /dev/sdc3. The following commands will format and mount this space as an ISO partition.

```
[root@MARLINSPIKE ~]# mkfs.ext3 /dev/sdc3
mke2fs 1.39 (29-May-2006)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
29491200 inodes, 58952233 blocks
2947611 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=0
1800 block groups
32768 blocks per group, 32768 fragments per group
16384 inodes per group
Superblock backups stored on blocks:
32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
4096000, 7962624, 11239424, 20480000, 23887872

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
This filesystem will be automatically checked every 32 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
[root@MARLINSPIKE]# mkdir /mnt/iso/
[root@MARLINSPIKE]# mount -t ext3 /dev/sdc3 /mnt/iso/
[root@MARLINSPIKE]# echo &quot;/dev/sdc3 /mnt/iso ext3 defaults 1 1&quot; &gt;&gt; /etc/fstab
[root@MARLINSPIKE]# xe-mount-iso-sr /mnt/iso -o bind
```

Now the new ISO partition shows up in my XenCenter console.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/xenserver-creating-an-iso-partition-on-dom0/image.png" width="244" height="110" alt="image">

**Sources:**  
+ Citrix Forums: Thread: Dom0 Partitions - `http://forums.citrix.com/message.jspa?messageID=1002446`
+ Citrix Forums: Thread: xe sr-create, local ISO SR on larger drive - `http://forums.citrix.com/thread.jspa?threadID=240181#)`
+ [**How to add an additional local disk to your XenServer 5.5 host**](http://web.archive.org/web/20110926042557/http://www.xendesktopmaster.com/how-to-add-an-additional-local-disk-to-your-xenserver-5-5-host/){:target="_blank"}
+ XenServer create local ISO Repository (LVM) - `http://linuxnet.ch/groups/linuxnet/wiki/28e6d/XenServer_create_local_ISO_Repository_LVM.html`
+ How To Re-partition a Xen Virtual Machine Using GParted LiveCD - `http://support.citrix.com/article/CTX116105`
+ [**LinuxQuestions.org: Having problems mounting hd. (mount: you must specify the filesystem type**)](https://web.archive.org/web/20110201011556/http://www.linuxquestions.org:80/questions/linux-newbie-8/having-problems-mounting-hd-mount-you-must-specify-the-filesystem-type-188956/){:target="_blank"}

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*