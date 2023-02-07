---
layout: post
title: Auto logoff Citrix users
date: 2009-05-22
tags: [Administration, PowerShell, Scripting, XenApp]
---
> NOTE: Have XenApp 6.x and PowerShell? I revisited this problem in XenApp 6.5 with a new PowerShell script. [Read about it here](/2017-03-07-drain-users-from-a-xenapp-server/)

I recently came across a situation where I needed to clear users off a server in production for testing.  Typically, we assign a load evaluator to keep new users from connecting to it and watch the server.  When a user's session becomes disconnected, we log the user off.  This can take a while depending on how busy the server is and is about as exciting as watching grass grow.  So, I thought "PowerShell can fix this problem!"  Here's the result.

```posh
# ==============================================================================================
# NAME: CheckXenAppIsFree.ps1
# AUTHOR: Alain Assaf
# DATE : 05/21/2009
#
# COMMENT: Script that will run on a locked down server and automatically disconnect users
# until there are no more sessions.
# USAGE: .\CheckXenAppIsFree.ps1
# ==============================================================================================

# Prompt for server to watch
if (($citrixserver -eq $null) -or ($citrixserver -eq "")){
    $citrixserver=$(Read-Host "Enter Server to watch until no sessions exist.")
}

# Setup Farm object
$MetaFrameWinAppObject = 3
$MetaFrameWinFarmObject = 1
$myFarm = New-Object -com MetaFrameCOM.MetaFrameFarm
$myFarm.Initialize($MetaFrameWinFarmObject)

# Checking if user is a Citrix administrator
If ($myFarm.WinFarmObject.IsCitrixAdministrator -eq 0) {
    Write-Output "You must be a Citrix admin to run this script`n"
}

# Checking that the Citrix server exists
$MetaframeServers = $myFarm.Servers | Sort-Object -property ServerName | ? { $_.ServerName -like $citrixserver}
If ($MetaframeServers -eq $null) {
    Write-Host "Invalid Citrix server" -ForegroundColor Red
    Write-Host "Exiting CheckXenAppIsFree" -ForegroundColor Red
    break
}

# Getting the currently assigned load evaluator
$le = $MetaframeServers.AttachedLE
$le.loaddata(1)
$LEName = $le.LEName
$XenAppServerName = $citrixserver.ToUpper()

# If the LE isn't set to Lockdown, then this script will exit
If ($LEName -eq 'Lockdown') {
    Write-Host "$XenAppServerName has '$LEName' attached." -ForegroundColor White
} else {
    Write-Host "$XenAppServerName does not have a Lockdown LE attached. It has '$LEName' attached." -ForegroundColor White
    Write-Host "Exiting CheckXenAppIsFree" -ForegroundColor Red
    break
}

# Get the current session count
$sessCount = $MetaframeServers.SessionCount

# Check for disconnected users. If none, wait 10 minutes and check again
# If there are disconnected users. Log them off. Continue until there are no more logged in sessions.
while ($sessCount -ge 0) {
    $MetaframeServers = $myFarm.Servers | Sort-Object -property ServerName | ? { $_.ServerName -like $citrixserver}
    $sessCount = $MetaframeServers.SessionCount
    Write-Host "Current session Count = $sessCount on $XenAppServerName" -ForegroundColor White
    if ($sessCount -eq 0) { 
        Write-Host "$XenAppServerName is free of users" -ForegroundColor White
        break
    } else {
        $disconnected = @()
        Write-Host "Checking for disconnected users on $XenAppServername" -ForegroundColor White
        $disconnected = @($myfarm.Sessions | Where-Object {$_.SessionState -eq 5 -and $_.ServerName -eq $citrixserver})
        if ($disconnected[0] -eq $null) {
            Write-Host "There are no currently disconnected sessions on $XenAppServerName" -ForegroundColor White
            Write-Host "Waiting 10 minutes" -ForegroundColor Red
            Start-Sleep -Seconds 600
        } else {
            Write-host "Logging off disconnected users from $XenAppServerName" -ForegroundColor White
            $i = 0
            foreach ($user in $disconnected) {
                $namedUser = $disconnected[$i].Username
                write-host "Logging off $namedUser" -ForegroundColor White
                $user.Logoff($false)
                $sessCount--
                $i++
            }
        }
    }
}
```

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*