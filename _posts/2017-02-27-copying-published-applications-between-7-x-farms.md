---
layout: post
title: Copying Published Applications Between 7.x Farms
date: 2017-02-27
readtime: true
tags: [Administration, Citrix XenApp, PowerShell, Scripting]
thumbnail-img: /assets/img/copying-published-applications-between-7-x-farms/copy1.jpg
share-img: /assets/img/copying-published-applications-between-7-x-farms/copy1.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/copying-published-applications-between-7-x-farms/copy1.jpg" 
    alt="dog">

# Intro #
We are in the midst of migrating to a XenApp 7.9 environment from a 6.5 environment. There are many pain points associated with this sort of transition:
<ol>
	<li>Lack of Worker Groups (similar functionality returns in XenApp 7.12)</li>
	<li>Organizing your existing application deployment around Machine and Delivery groups</li>
	<li>Revisiting every published application in relation to user access and how Delivery groups are leveraged</li>
	<li>Recreating all your published applications</li>
</ol>
In my current 6.5 environment we have over 240 published applications. Recreating these in the new 7.9 farm is nightmarish. Luckily, <a href="https://www.linkedin.com/in/meritumshaun/" target="_blank">Shaun Ritchie</a> (from <a href="https://meritum.cloud/" target="_blank">Metrium Cloud</a>) has provided the Citrix community a great script that migrates (with icons and user accounts) XenApp 6.5 published applications to a XenApp 7.x farm. You can see the script on his blog post "<a href="https://meritum.cloud/xenapp-6-x-to-xenapp-7-x-app-migration-script/" target="_blank">XenApp 6.x to XenApp 7.x App Migration Script</a>".

This script does a lot of heavy lifting and makes a migration much easier. What do you do if you have multiple 7.x farms? I thought it would be trivial to copy the newly migrated 6.5 applications from one 7.x farm to another, but this was not the case. I used Shaun's script as a base and re-worked it to allow the copying of apps from one XenApp 7.x farm to another.
<h1>The Script</h1>
You can get the script from <a href="https://github.com/alainassaf/copy-xa7apps.git" target="_blank">GitHub</a>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*