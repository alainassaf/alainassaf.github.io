---
layout: post
title: Drain users from a XenApp Server
date: 2017-03-07
readtime: true
tags: [Administration, Citrix XenApp, PowerShell, Scripting]
thumbnail-img: /assets/img/drain-users-from-a-xenapp-server/drain-xaserver.jpg
share-img: /assets/img/drain-users-from-a-xenapp-server/drain-xaserver.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/drain-users-from-a-xenapp-server/drain-xaserver.jpg" 
    alt="ski">

# Intro #
Citrix has provided XenApp administrators several tools to control server access. Applying <a href="https://www.citrix.com/blogs/2013/03/14/how-to-put-xenapp-servers-to-maintenance-mode/" target="_blank"><b>load evaluators and login modes</b></a> allows us to establish reboot and maintenance schedules. At times, however getting users off a server becomes urgent. 
In this post we'll cover a script designed to automatically drain users off a XenApp server with different levels of "aggression".

# Scenario #
Your ever-intrepid security team informs you that a user on one of your Citrix servers went to a malicious web site. While everyone is reasonably sure that the web security filters prevented anything bad from happening, you err on the side of caution and implement a standard procedure to remove the system from production, perform a security scan, get the connected users off, and reboot the server (because you've naturally implemented PVS and rebooting will remove any changes that have occurred since the image was last sealed). Depending on your <a href="https://support.citrix.com/article/CTX140320" target="_blank"><b>idle and disconnection policies</b></a>, this could take a while. If only there was a way to get users off a system in a timely manner without inconveniencing them too much. Oh wait...PowerShell

# Why So Aggressive? #
The purpose of the drain-xaserver script is to look for disconnected ICA sessions and log them off your server. But what if your users are busy working away or if you have liberal idle session timeouts? Well, in order to add some urgency to this script, I included a switch parameter that determines how long a session can be idle before it is disconnected and then logged off.

{% gist 74f48dc5142aec78300decf723d66fab %}

If you choose <span style="color:#339966;">Green</span><span style="color:#808080;"> (the default value), </span>then your normal idle timeouts will occur. If you choose <span style="color:#d8e300;">Yellow</span>, the idle timeout gets reduced to 30 minutes. If you choose <span style="color:#ff0000;">Red</span>, the idle timeout is further reduced to 15 minutes. You can, of course, modify these settings to be longer or shorter. Just to be clear, these settings will override your Farm-wide idle session timeouts on the server you run this script against only while the script is running.

{% gist 522b4c42caf7f92822b2fba942ca8371 %}

This script checks that a compatible Logon Mode was assigned to the server before sessions get logged off. In this case, the following Logon Modes will allow the script to proceed:
<ul>
	<li>ProhibitLogOns</li>
	<li>ProhibitNewLogOns</li>
	<li>ProhibitNewLogOnsUntilRestart</li>
</ul>
Otherwise, the script will not run.

# While True...Do Stuff #

{% gist b4090cfad22d76f79649baa8ac48f30a %}

The script checks the number of ICA sessions and as long as it isn't zero, keeps checking every 10 minutes (adjust for your environment) for new disconnected sessions. If you have set a <span style="color:#ff0000;">Red</span> or <span style="color:#d8e300;">Yellow </span>aggression level, line 15 (above) shows how the script calculates the user's idle time. The LastInputTime value comes from running Get-XASession with the "-full" switch. This queries Citrix for session details that are usually omitted when using Get-XASession.

# The Script #
I went through a lot of revisions while testing, so please let me know if you run into any issues using this script in a XenApp 6.x environment. You can get the script from <a href="https://github.com/alainassaf/drain-xaserver" target="_blank"><b>GitHub</b></a>.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*