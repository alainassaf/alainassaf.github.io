---
layout: post
title: Downloading Citrix App Layering (Unidesk)
date: 2017-04-11
readtime: true
tags: [App Layering, Application, Citrix Cloud, Layering, Unidesk, Virtualization]
thumbnail-img: /assets/img/downloading-citrix-app-layering-unidesk/unidesk_ww.jpg
share-img: /assets/img/downloading-citrix-app-layering-unidesk/unidesk_ww.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk_ww.jpg" 
    alt="ww">

# Intro #
About a week ago (April 3, 2017), Citrix released <a href="https://www.citrix.com/blogs/2017/04/03/now-available-citrix-app-layering" target="_blank"><b>App Layering 4.1.0</b></a>. This is the slightly rebranded <b>Unidesk</b> and Citrix is emphasizing (over-emphasizing based on some comments) that it...

<blockquote>Achieves app compatibility greater than 99.5%</blockquote>

I find it interesting that out of the gate, Citrix is positioning App Layering as a direct competitor/replacement for Microsoft App-V, which I believe has more success supporting many different types of applications (this is based on my years of experience with App-V and minutes spent with Unidesk :) ). I always viewed Unidesk as a more comprehensive approach to delivering a fully realized, layered solution. But let's face it, most Citrix administrators/engineers have to become application analysts/re-packagers/engineers in order to make applications work in a Citrix deployment. This blog will not dive into using App Layering, but instead will try and shine some light on actually downloading this "new" product from Citrix

# Requirements #

###### Note: The following is based on discussions with our Citrix sales engineer and the Unidesk website ######

<ul>
	<li>You must be active on <a href="https://www.citrix.com/support/programs/customer-success-services-select.html" target="_blank"><b>Select Service</b></a> to use it.</li>
	<li>You must have platinum licensing if you wish to run App Layering on-premises.</li>
	<li>You must have a one of the following hypervisors
<ul>
	<li>Citrix XenServer (6.5, 7.0, 7.1)</li>
	<li>Microsoft Azure</li>
	<li>Microsoft Hyper-V (at least 2012 R2)</li>
	<li>Nutanix Acropolis</li>
	<li>VMware VSphere (5.5.x, 6.0.x, 6.5.x)</li>
</ul>
</li>
	<li>SMB Network File Share Protocol</li>
	<li>A network connection of 10GB between the Layering appliance and the file share</li>
	<li>One of the following publishing platforms
<ul>
	<li>Citrix MCS for Nutanix AHV</li>
	<li>Citrix MCS for vSphere</li>
	<li>Citrix MCS for XenServer</li>
	<li>Citrix PVS 7.1, 7.6 - 7.9, 7.11 - 7.12 with recommended network speeds to the PVS Store of 10 GB.</li>
	<li>Citrix XenApp and XenDesktop 6.5, 7.0 - 7.12</li>
	<li>Microsoft Azure, with recommended network speeds to the Azure publishing location of 10 GB.</li>
	<li>VMware Horizon View 6.x, 7.0.x (Note: View Persona Management is not supported with Elastic Layering)</li>
</ul>
</li>
	<li>A <a href="//citrix.cloud.com" target="_blank"><b>Citrix Cloud</b></a> Account</li>
</ul>

# How to Download #

Login into your Citrix Cloud Account
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk1.png" 
    alt="unidesk1">

Go down to Available Services and click Request Trial on App Layering
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk21.png" 
    alt="unidesk21">

Once you receive an email from Citrix Cloud, log back into your account. Find App Layering and click Manage
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk3.png" 
    alt="unidesk3">

On the App Layering screen, click Get Started
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk41.png" 
    alt="unidesk41">

Install the Cloud Connector (This will be required if you do not have local resources to run the appliance).
###### Note: Installing the Cloud Connector will affect your existing Citrix Studio install. You will have to uninstall the Cloud Connector and Studio and then reinstall Studio if you installed the Connector accidentally or just to try it out. ######

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk5.png" 
    alt="unidesk5">

Download the appliance
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk6.png" 
    alt="unidesk6">

Install and Configure the Appliance
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk7.png" 
    alt="unidesk7">

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/downloading-citrix-app-layering-unidesk/unidesk8.png" 
    alt="unidesk8">

# That's all for now #
I won't cover the install and configuration of the appliance at this time. Hopefully, this helps shed some light on actually downloading the appliance. 

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*