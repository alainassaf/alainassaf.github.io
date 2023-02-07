---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 2
date: 2009-03-26
tags: [Business Intelligence, Citrix, PowerShell, Scripting]
---

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

For the next part of this series, I just want to get the data I'm looking for into text files for later use.  Here's the modified PoSH script (<em>with comments</em>) with the text file output:

```posh
# Create log files
$logonly = $false

# The $logonly variable will be set to false to prevent output to the screen.
# If you wish to get visual feedback while the script runs, then set it to $true.
$sessLog = "w:\qfarm\totSessions.txt"
$appLog = "w:\qfarm\top20apps.txt"
$serverLog = "w:\qfarm\totSessionsServer.txt"
$multiLog = "w:\qfarm\multipleSessions.txt"
if (Test-Path $sessLog) {New-Item $sessLog -Type file -force | Out-Null}
if (Test-Path $appLog) {New-Item $appLog -Type file -force | Out-Null}
if (Test-Path $serverLog) {New-Item $serverLog -Type file -force | Out-Null}
if (Test-Path $multiLog) {New-Item $multiLog -Type file -force | Out-Null}

# Here, I'm defining several different text files depending on its data.  
# This could just as easily be one file, but for my purposes, 
# it will be easier if they are separate.  The intention is to run this 
# script periodically, and dump the # info into a database, so we do not need 
# to keep the data.  Therefore, I am forcing the scripts to clear out old 
# data before we write to them again.
$livesessions = 0
$disconnected = 0
$farm = New-Object -com "MetaframeCOM.MetaframeFarm"
$farm.Initialize(1)

# Load Up Array for a snapshot of current sessions
$sessionAry = @($farm.Sessions | select UserName,AppName,ServerName,SessionState)

foreach ($sess in $sessionAry) {
if ($sess.SessionState -eq "5") {$disconnected = $disconnected + 1}
else {$liveessions = $livesessions++}
}

if ($logonly) {
    Write-Host "The number of active citrix sessions is" $livesessions -fore red
}
if ($logonly) {
    Write-Host "The numbrer of disconnected citrix sessions is" $disconnected -fore red
}
Add-Content $sessLog "$livesessions"
Add-Content $sessLog "$disconnected"

if ($logonly) {Write-Host " "}

# Displays a list of published apps and the number of users on each
if ($logonly) {
    Write-Host "Total users on top 20 citrix applications" -fore yellow
}
$sessionAry | group AppName | sort Count -desc | select Count,name -first 20 | ft -auto | Out-File $appLog

if ($logonly) {Write-Host " "}

# List of citrix servers and total number of sessions on each one
if ($logonly) {
    write-host "Total sessions on each citrix server" -fore yellow
}
$sessionAry | group ServerName | sort name | select Count,Name | ft -auto | Out-File $serverLog

if ($logonly) {write-host " "}

# To see which users have more than one session open
if ($logonly) {
    write-host "First 20 Users with more than one citrix session" -fore yellow
}
$sessionAry | group UserName | Sort Count -desc | select Count,Name -first 20 | ft -auto | Out-File $multiLog
```

That's it.  I'll cover importing this data into a database in the next post in this series.

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*