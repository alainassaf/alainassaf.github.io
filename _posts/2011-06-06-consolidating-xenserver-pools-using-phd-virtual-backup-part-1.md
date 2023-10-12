---
layout: post
title: Consolidating XenServer Pools Using PHD Virtual Backup
subtitle: Part 1
date: 2011-06-06
tags: [Administration, Citrix, PHD Virtual Backup, Virtualization, XenServer]
---
<a href="https://citrix.com/" target="_blank"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;float:left;padding-top:0;border:0;margin:0 15px 0 5px;" title="xenserver" src="/assets/img/icons/xenserver.jpg" alt="xenserver" width="129" height="122" align="left" border="0" />

### Scenario

My team is nearing the end of a migration from physical XenApp servers to virtual XenApp servers running on XenServer.  During the testing phase of the new XenApp farm, we found that CPU utilization was excessive and we eventually determined that we had different CPU architectures in some of the servers (with the same model number) so we had to split our XenServer Pool.  I mentioned this issue in a previous Tweet:
   
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Using HP DL385 G5&#39;s and having CPU issues? Check CPU stepping level. Known issue with CPU Stepping 2 and Linux. Can e…http://lnkd.in/8yWAZt</p>&mdash; Alain Assaf (@alainassaf) <a href="https://twitter.com/alainassaf/status/43051141646385152?ref_src=twsrc%5Etfw">March 2, 2011</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

We have since replaced the CPU’s in the newer servers and now we wish to consolidate our pools into one group.  Currently the two pools are split along functionality, with one being for production and the other being for development.  We are using PHD Virtual Backup to archive PVS master images, Citrix Streaming profilers, and some other systems only in the development pool.

This blog series will detail the steps I went through in order to consolidate all our VM’s and Servers into one pool.

<strong>Install PHD Virtual Backup</strong>

The first step is to install PHD into the production XenServer pool so we can perform restores into this pool.
<ol>
	<li>Download the PHD Virtual Backup for Citrix trial from `http://www.phdvirtual.com/downloadtrial_citrix`</li>
	<li>Extract the compressed file and launch the phdvb.msi on a computer with XenCenter installed.</li>
	<li>Accept the license agreement and click Finish when the Installation wizard completes.</li>
	<li>Restart XenCenter</li>
	<li>Import the PHD virtual appliance (PHDVBA.xva)</li>
	<li>If the appliance does not get an IP, you must go to the console and set a static or dynamic IP by typing Ctrl-N.</li>
	<li>Right-click the appliance and select the Console</li>
</ol>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image.png" width="322" height="254" alt="image">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image1.png" width="644" height="389" alt="image1">

Click Configuration and setup the backup agent…

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image2.png" alt="image2">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image3.png" alt="image3">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image4.png" alt="image4">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image5.png" alt="image5">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image6.png" alt="image6">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/consolidating-xenserver-pools-using-phd-virtual-backup-part-1/image7.png" alt="image7">

In my next post we will test a backup from one pool to a restore in the other pool.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*