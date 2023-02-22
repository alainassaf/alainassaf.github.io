---
layout: post
title: E-mail Alert when your TS logins are disabled
subtitle: Wisdom-Fu
date: 2010-05-28
tags: [Administration, Citrix, Reporting, Monitoring, Res Wisdom, XenApp]
---
###### Note: This blog contains screenshots and references to an older version of [**Ivanti Automation**](https://www.ivanti.com/products/automation) which was developed by a company called **RES Software** that was aquired by Invanti.

<strong>Introduction</strong>

XenApp server maintenance is a necessary part of managing a Citrix farm (unless you are using Provisioning Services?).  We use Wisdom (which I will detail in a later post) to perform an elaborate, reoccurring maintenance reboot job on our servers.  Part of the maintenance job is to disable Terminal Server logins when the job starts and enable them when the job completes.  Naturally, Murphy’s Law is in effect all the time when it comes to Citrix servers, so the maintenance job will not complete on a couple of servers and they will have their logins disabled when users start to login.  For this post, I’m going to detail a simple Wisdom module that you can schedule to run ‘in the morning’ that will send you an e-mail alert for any servers that have their logins disabled.

***
<strong>Wisdom Module Overview</strong>

The module consists of 2 actions:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/e-mail-alert-when-your-ts-logins-are-disabled/image7.png" width="644" height="112" alt="image7">

With these module parameters:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/e-mail-alert-when-your-ts-logins-are-disabled/image8.png" width="644" height="140" alt="image8">

<ul>
	<li>SERVERNAME will be set to the %COMPUTERNAME% environment variable of the server the module is run against.</li>
	<li>SecurityContext is used for the E-mail task</li>
	<li>LoginsDisabled is a conditional parameter that we use to determine whether to send an e-mail or not.</li>
</ul>
<hr /><strong>Wisdom Module Detail</strong>

<em><span style="text-decoration:underline;">Query Registry Settings</span></em>

The first task simply examines the <em>WinStationsDisabled</em> value for the following registry key:
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`


If the value is 1, then we’ll set our Wisdom parameter LoginsDisabled to 1, otherwise, it is 0.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/e-mail-alert-when-your-ts-logins-are-disabled/image9.png" width="636" height="484" alt="image9">

<span style="text-decoration:underline;"><em>Send E-mail</em></span>

The second task runs if the LoginsDisabled parameter is equal to 1, otherwise skip this task.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/e-mail-alert-when-your-ts-logins-are-disabled/image10.png" width="635" height="484" alt="image10">

Here are the e-mail settings so you can see how the SERVERNAME parameter is used:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/e-mail-alert-when-your-ts-logins-are-disabled/image11.png" width="640" height="484" alt="image11">

Building block available ~~here~~.  You can also follow more discussion at the ~~Res Software User Group forums~~.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*