---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 1
date: 2009-03-26
tags: [Business Intelligence, Citrix, Reporting, PowerShell, Scripting, SQL, Visual Studio]
---
> This post is part of a 6 part series.  Jump to [\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

# Intro #

There's quite a bit of data that we can gather via queries to MFCom.  With this series I want to demonstrate how to use PowerShell, MS SQL, SQL Reporting Services and Visual Studio to gather real-time stats and present them in a dashboard that is easy to read and even easier to present to management.

I'm presenting today an edited PowerShell script that I grabbed from <a href="http://web.archive.org/web/20110830203202/http://synjunkie.blogspot.com:80/2008/12/powershell-retrieving-useful-citrix.html"><b>SynJunkie's PowerShell - Retrieving Useful Citrix Stats</b></a>.  Syn is also writing a <a href="http://web.archive.org/web/20091014161106/http://synjunkie.blogspot.com/2009/03/abusing-citrix-part-1.html"><b>hacking Citrix</b></a> series of articles that is proving to be very interesting.

Here's the original script:

```posh
#Count-CitrixSession.ps1

# Count citrix sessions and other useful info
$farm = new-Object -com "MetaframeCOM.MetaframeFarm"
$farm.Initialize(1)

# Displays a list of published apps and the number of users on each
write-host "Total users on each citrix application" -fore yellow
$farm.sessions | select UserName,AppName | group AppName | Sort Count -desc | select Count,Name | ft -auto
write-host "The number of current citrix sessions is" $livesessions -fore red
write-host " "

# List of Citrix servers and total number of sessions on each one
write-host "Total sessions on each citrix server" -fore yellow
$farm.sessions | select ServerName,AppName | group ServerName | sort name | select Count,Name | ft -auto
write-host " "

# To see which users have more than one session open
write-host "First 15 Users with more than one citrix session" -fore yellow
$farm.sessions | select UserName,AppName | group UserName | Sort Count -desc | select Count,Name -first 15 | ft -auto
```

This script is useful, but I found that it took several minutes to run in our environment due to iterating through all our Citrix sessions 3 times.  To speed the script up and ensure the data did not change as the script was running I loaded everything into an array and then replicated the data output.

```posh
$livesessions = 0
$disconnected = 0
$farm = New-Object -com "MetaframeCOM.MetaframeFarm"
$farm.Initialize(1)

# Load Up Array for a snapshot of current sessions in Citrix
$sessionAry = @($farm.Sessions | select UserName,AppName,ServerName,SessionState)
foreach ($sess in $sessionAry) {
    if ($sess.SessionState -eq "5") {$disconnected = $disconnected + 1}
    else {$liveessions = $livesessions++}
}
Write-Host "The number of active citrix sessions is" $livesessions -fore red
Write-Host "The numbrer of disconnected citrix sessions is" $disconnected -fore red
Write-Host " "

# Displays a list of published apps and the number of users on each
Write-Host "Total users on top 20 citrix applications" -fore yellow
$sessionAry | group AppName | sort Count -desc | select Count,name -first 20 | ft -auto
Write-Host " "

# List of citrix servers and total number of sessions on each one
write-host "Total sessions on each citrix server" -fore yellow
$sessionAry | group ServerName | sort name | select Count,Name | ft -auto
write-host " "

# To see which users have more than one session open
write-host "First 20 Users with more than one citrix session" -fore yellow
$sessionAry | group UserName | Sort Count -desc | select Count,Name -first 20 | ft -auto
```

Now the script runs 3 times as fast.

> This post is part of a 6 part series.  Jump to [\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*