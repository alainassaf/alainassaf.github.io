---
layout: post
title: Cleanup Reboot script
date: 2017-03-02
tags: [Administration, PowerShell, Reporting, Monitoring, XenApp]
thumbnail-img: /assets/img/cleanup-reboot-script/cleanup-reboot.jpg
share-img: /assets/img/cleanup-reboot-script/cleanup-reboot.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-cleanup-reboot-script/cleanup-reboot.jpg" 
    alt="happykid">

# Intro #
If your environment leverages Provisioning Services and is fairly large, you may run into issues with servers coming up but not allowing any users to login. The fix we implemented is to reboot those servers after it's clear that they will not allow users to login (usually sometime mid-morning when most of our users have logged in). This was  a manual process and I wanted to find a way to automate this until we could change our reboot schedules or find a resolution. In this post we'll explore using a script to catch these unresponsive servers.
<h1>Scenario</h1>
Your reboot script ran last night and you find that the next day there are a handful of servers that report to the farm, but do not have any users attached. You've checked the obvious problems/issues and the event log isn't any help. The only solution seems to be another reboot. Once the system comes up, users are able to connect.

In order to automate the reboots, we have to determine the criteria that identifies a server as having a problem and not just an idle server.
<ol>
	<li>It is mid-morning and most of our users are logged in and using Citrix. The server has no users connected even though the load evaluators and login behavior should allow users to connect.</li>
	<li>The server is marked as 'Online' when using the PowerShell Cmdlet get-xaserver.</li>
	<li>The server is a not in our excluded list (more on this below)</li>
	<li>The server has a high load even though it is hosting no users.</li>
</ol>
<h2>I got <del>one</del> two words for you...Worker Groups</h2>

{% gist c0fa0454bbc5e6816a361b4ebe9e3255 %}

You will have to edit the <span style="color:#ff0000;">$WORKERGROUPS</span> variable to list the Citrix Worker Groups (WG) you wish to check for zero-user servers. The list can be separated by commas. Optionally you can also edit <span style="color:#ff0000;">$</span><span class="pl-smi"><span style="color:#ff0000;">EXCLUDESERVERS</span> to have the script ignore servers you do not want checked for zero-users.</span>

The script will iterate though the servers in each WG. This is done with 2 nested <span style="color:#ff0000;">foreach</span> loops.

{% gist 42bf29394a430c3a5a20e4ff74bd59e5 %}

The script confirms the Worker Group is valid and then iterates through the servers in the WG. It checks that the server is considered Online and not a member of the <span style="color:#ff0000;">$EXCLUDESERVERS</span> list. It then confirms the server has no users connected and then checks to see if the load is greater than or equal to 3500. In our farm, this was a common characteristic for these zero-user servers. You may need to change this to better reflect your environment. I setup a scheduled task to run this script every day around 10Am and it has automated an administrative task freeing me up to write blogs about PowerShell.

<strong>NOTE</strong> that this script does not actually reboot the server. We have another script that runs all day looking for servers that are set to <span style="color:#339966;">ProhibitNewLogOnsUntilRestart</span>. This secondary script checks that no users are logged into a server set to <span style="color:#339966;">ProhibitNewLogOnsUntilRestart</span> and then reboots it. You could edit the script above to add this.
<h2>What about reporting?</h2>

{% gist bd26a462967ca0524ae4484210276071 %}

If you wish to receive an email report of which servers were set to <span style="color:#339966;">ProhibitNewLogOnsUntilRestart</span> uncomment the above lines in the script. You will have to provide an <span style="color:#0000ff;">SMTP</span> server and a single or list of email addresses. Here's an example of the email report:

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/cleanup-reboot-script/cleanup-reboot.png" 
    alt="happykid">

# The Script # 
You can download the script from <a href="https://github.com/alainassaf/cleanup-reboot" target="_blank" rel="noopener"><b>Github</b></a>.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*