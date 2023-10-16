---
layout: post
title: Run a tool against Citrix Workergroup Servers
date: 2017-02-08
readtime: true
tags: [Administration, Citrix, PowerShell, Scripting]
thumbnail-img: /assets/img/run-a-tool-against-citrix-workergroup-servers/interestingman.jpg
share-img: /assets/img/run-a-tool-against-citrix-workergroup-servers/interestingman.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/run-a-tool-against-citrix-workergroup-servers/interestingman.jpg" 
    alt="interestingman">

# Intro #
In my new job, I get to work on a dedicated Citrix team again and I'm really enjoying it. I get the opportunity to work collaboratively with a group of experienced Citrix Admins/Engineers and also get the chance to do a lot of PowerShell. Recently, we had to run <a href="https://helgeklein.com/" target="_blank"><b>Helge Klein's</b></a> excellent <a href="https://helgeklein.com/free-tools/delprof2-user-profile-deletion-tool/" target="_blank"><b>Delprof2</b></a> against a set of servers because of space issues. After fixing the issue, I thought it would be a good chance to stretch my PowerShell skills and enhance a tool the team uses.

### Original Script ###
{% gist af85324dfbd043f169781872b09d8500 %}

The original script runs fine. It just runs against the entire farm which in this case is over 700 servers. I wanted to create a script for our XenApp 6.5 environment and leverage <a href="http://knowcitrix.com/worker-group/" target="_blank"><b>Worker Groups</b></a> to group our servers. I also wanted to try a graphical interface for the script.

### PowerShell...GUI...what's wrong with you? ###
I know, I know. Using a GUI with a PowerShell script is not typical, but I felt it was the best way to present a list of Worker Groups. Your Citrix environment may be smaller or not using that many worker groups, so displaying a list in the console may make more sense. I found <a href="https://learn.microsoft.com/en-us/powershell/scripting/samples/selecting-items-from-a-list-box?view=powershell-7.3" target="_blank"><b>this post</b></a> which outlined how to do a list box in PowerShell.

First, I modified the dimensions of the parts of the list box so it would display all my worker groups.

{% gist 58565d891e2a7bfaabfe13c16ee550e0 %}

Then, I populated the list box with worker groups using get-xaworkergroup

{% gist 776332386922aec52b9c2d5b657cb387 %}

Finally, I display the List box and wait for the user to select a Worker Group and click OK or Cancel and stop the script.

{% gist 27fa6430208abfcb0512005e24c5e51d %}

If the user does pick a worker group and clicks OK, then we iterate through the servers in the Work Group and run delprof2.exe against them. This is where you could implement your own tool or procedure.

{% gist d67470175e0403774d0313c7ad0aac70 %}

Here's the list box:
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/run-a-tool-against-citrix-workergroup-servers/wg1.png" 
    alt="wg1.png">

Selecting a Worker group and clicking OK, will run delprof2.exe against all the servers in the WG.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/run-a-tool-against-citrix-workergroup-servers/wg2.png" 
    alt="wg2.png">

### The script ###

You can get the script from <a href="https://github.com/alainassaf/run-delprof2" target="_blank">Github</a>.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*