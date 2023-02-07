---
layout: post
title: Friday Script Blitz
date: 2017-03-10
readtime: true
tags: [Administration, Citrix XenApp, PowerShell, Scripting]
thumbnail-img: /assets/img/powershell-friday-script-blitz/everyone-gets-powershell.jpg
share-img: /assets/img/powershell-friday-script-blitz/everyone-gets-powershell.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-friday-script-blitz/everyone-gets-powershell.jpg" 
    alt="opra">

# Intro #
In my current position I'm getting to do a lot of PowerShell scripting. Typically these are quick scripts for maintenance or finding information about our Citrix environment. I'm posting several here to share.
###### NOTE: These scripts were written against a XenApp 6.5 environment. ######

## get-xacmdln.ps1 ##
Lists active published applications' command lines and working directories based on a search word.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-friday-script-blitz/psscriptblitz1a.png" 
    alt="psscriptblitz1a">

Get it from <a href="https://github.com/alainassaf/get-xacmdln" target="_blank"><b>GitHub</b></a>.

## get-xawgapps.ps1 ##
Lists active published applications from a designated XA 6.x Worker Group

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-friday-script-blitz/psscriptblitz1b.png" 
    alt="psscriptblitz1b">

Get it from <a href="https://github.com/alainassaf/get-xawgapps" target="_blank"><b>GitHub</b></a>

## get-xasessions.ps1 ##
Displays total, active, and disconnected sessions from a XA 6.x farm

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-friday-script-blitz/psscriptblitz1c.png" 
    alt="psscriptblitz1c">

Get it from <a href="https://github.com/alainassaf/get-xasessions" target="_blank"><b>GitHub</b></a>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*