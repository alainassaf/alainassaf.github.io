---
layout: post
title: Friday Script Blitz 2
date: 2017-04-28
readtime: true
tags: [Administration, PowerShell, Scripting]
thumbnail-img: /assets/img/friday-script-blitz-2/scripts-powershell-scripts-everywhere1.jpg
share-img: /assets/img/friday-script-blitz-2/scripts-powershell-scripts-everywhere1.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/friday-script-blitz-2/scripts-powershell-scripts-everywhere1.jpg" 
    alt="toystory">

# Intro #
In my current position I’m getting to do a lot of PowerShell scripting. Typically these are quick scripts for maintenance or finding information about our Citrix environment. I’m posting several here to share.

###### NOTE: These scripts were written for a XenApp 6.5 environment ######

## check-deedrive.ps1 ##
Iterates though all XenApp Servers in the farm and checks that the D: drive is formatted. I wrote this because we found some existing provisioned servers that had unformatted D: drives attached.

Get it from <a href="https://github.com/alainassaf/check-deedrive" target="_blank" rel="noopener noreferrer"><b>GitHub</b></a>

## count-usrprof.ps1 ##
Iterates though user's profile directories and counts number of files in specified sub directory. It produces a CSV report (if the file count is above a threshold you set in the script) and also counts the total number of profiles. Useful for confirming your profile management solution is working as expected.

Get it from <a href="https://github.com/alainassaf/count-usrprof" target="_blank" rel="noopener noreferrer"><b>GitHub</b></a>

## clean-crashdumps.ps1 ##
Iterates though a list of servers and reports on crashdumps. EdgeSight and Windows can collect crashdumps and if you don't clear them off, the accumulate. This script will generate a CSV report and delete the dumps if the -delete switch is included.

Get it from <a href="https://github.com/alainassaf/clean-crashdumps" target="_blank" rel="noopener noreferrer"><b>GitHub</b></a>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*