---
layout: post
title: Installed Software Versions
subtitle: Using PowerShell
date: 2014-06-12
readtime: true
tags: [Documentation, PowerShell, Reporting, Monitoring, Scripting]
---
# Intro #
When you take over an existing environment, there are always surprises that keep one up at night. One of the most important applications that I deliver with my cloud environment was developed in-house. It was never designed to run in a multi-user environment and each installation is specific to a single user. This is a mess to say the least, but one of the largest issues is how to maintain, update, and easily deploy to new users. This post will begin with determining how many versions of this software are installed.

# First the Good News… #
I love starting with the good news. All the installs are on one XenApp server. This will allow us to iterate though some number of directories and query all the EXE’s for their version without making RPC’s or some other method of file query.

# Now the Bad News… #
Each install generates 3 (sort-of) random sub-directories, so there is no fixed pattern to where the main EXE resides. I say sort-of because at least they follow a pattern that we can rely on. RegEx to the rescue! The rest of the bad news is that all versions of the software under the latest will not automatically update. Thus I need this script to know how big a problem I have and which users I need to contact to update their software.

# TO DO’s #
I can add more fields to the CSV by doing an AD user lookup and getting an email address and/or phone number as I will have to contact these users to upgrade their software. This will require either Microsoft or Quest Active Directory PowerShell modules.

The Script

```posh
<#
.SYNOPSIS
Creates a list of users and their installed Software Version.
.DESCRIPTION
Creates a list of users and their installed Software Version.

It is recommended that this script be run as an admin.
.PARAMETER UserDirectory
Defaults to %HOMEDRIVE%\Users
Used to iterate through the Users directory and look for Software installs
.PARAMETER Outputfolder
Defaults to current folder location.
Enter a path to output the file.
.EXAMPLE
PS C:\PSScript > .\check-Softwareversion.ps1
Will use all default values.
$env:homedrive = C:

Output file will be in the current directory.
.INPUTS
None.  You cannot pipe objects to this script.
.OUTPUTS
No objects are output from this script.  This script creates a CVS document.
.NOTES
NAME: check-Softwareversion.ps1
VERSION: 1.00
CHANGE LOG - Version - When - What - Who
1.00 - 06/04/2014 - Inititail script - Alain Assaf
AUTHOR: Alain Assaf
LASTEDIT: June 4, 2014
.LINK
http://www.linkedin.com/in/alainassaf/
http://wagthereal.com
http://kevinpelgrims.com/blog/2010/05/10/add-help-to-your-own-powershell-scripts/
https://4sysops.com/archives/powershell-script-to-query-file-versions-on-remote-computers/
http://www.regexplanet.com
#>

Param(
[parameter(Position = 0, Mandatory=$False )]
[ValidateNotNullOrEmpty()]
[string]$UserDirectory="$env:homedrive\Users",

[parameter(Position = 1, Mandatory=$False )]
[ValidateNotNullOrEmpty()]
[string]$Outputfolder=".\"
)

#Constants
$Appdata = "AppData\Local\apps\2.0"
$verInfo = New-Object System.Collections.ArrayList
$datetime = get-date -format "MM-dd-yyyy_HH-mm"
$Software = "somesoftware.exe"

#Get user list from $UserDirectory
$UserFolders = Get-ChildItem $UserDirectory

foreach ($UserDir in $UserFolders) {
$User = $UserDir.Name
$UserAppdata = "$UserDirectory\$User\$AppData"
if (test-path $UserAppdata) {
$subdir1 = Get-ChildItem $UserAppdata | where {$_.name -match '\S{8}.\S{3}'}
$TempDir = "$UserAppdata\$subdir1"
$subdir2 = Get-ChildItem $TempDir | where {$_.name -match '\S{8}.\S{3}'}
$TempDir2 = "$TempDir\$subdir2"
$subdir3 = Get-ChildItem $TempDir2 | where {$_.name -like 'other..dir*'}
$SoftwareDir = "$TempDir2\$subdir3"
$SoftwareApp = Get-ChildItem $Softwaredir | where {$_.Name -eq $Software}
$SoftwareVersion = ($SoftwareApp.VersionInfo).FileVersion
$SoftwareFileName = ($SoftwareApp.VersionInfo).FileName
$verInfo += New-Object psObject -Property @{'User'=$user;'SoftwareDir'=$SoftwareDir;'SoftwareFilename'=$SoftwareFileName;'SoftwareVersion'=$SoftwareVersion}
}
}

#Write report to CSV file
$LogFileName = $Software + "_VersionInfo" + $datetime + ".csv"
$LogFile = $Outputfolder + $LogFilename
$verInfo | Export-Csv -Path $LogFile
```

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*