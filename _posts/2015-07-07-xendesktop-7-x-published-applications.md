---
layout: post
title: XenDesktop 7.x Published Applications
date: 2015-07-07
readtime: true
tags: [Administration, Application, PowerShell, Scripting, XenDesktop7.x]
thumbnail-img: /assets/img/xendesktop-7-x-published-applications/xendesktop7iscoming.jpg
share-img: /assets/img/xendesktop-7-x-published-applications/xendesktop7iscoming.jpg
---
<img 
	style="display: block;
		   margin-left: auto;
		   margin-right: auto;"
	src="/assets/img/xendesktop-7-x-published-applications/xendesktop7iscoming.jpg" 
	alt="stark">

# Intro #
Citrix XenDesktop 7 represents a major change from previous versions of Citrix. Those of us who have managed large XenApp-based farms with many published applications will find us rushing to scour the Internet for how-to's and Citrix documentation because everything "seems" very different.

You are not wrong...things are different, and if you are considering a move to XenDesktop 7.x, you owe it to yourself to document (in detail) what you have done in your current XenApp 6.5, 5.0, etc environment and examine how to do the same thing XenDesktop 7.x. In this blog post, we will look at a PowerShell script that will automate the creation of published applications in XenDesktop 7.6.

# Scenario #
I'm migrating a XenApp 6.5 / XenDesktop 5.6 environment to XenDesktop 7.6. In the old XenApp farm, users run a split Access database as a published application with many front-ends pointing to a single back-end. The idea of recreating 60+ published applications (each assigned to a single user) with the Citrix Studio GUI is <del>like a dream come true</del>...no I mean a nightmare.

I wanted to approach this with a script to automate the front-end creation and to minimize any user error. This could allow a lower-level admin or helpdesk member to create the front-end without opening up the Citrix Studio.

# The Script #
<small>NOTE: The new-brokerapplication cmdlet resides in the Citrix.Broker.Admin.V2 module. It should available on any system/workstation with the Citrix Studio installed.</small>

```posh
.SYNOPSIS
Takes a username or list of usernames and creates the same published application for each user.
.DESCRIPTION
Takes a username or list of usernames and creates the same published application for each user.

It is recommended that this script be run as an AD and Citrix admin. In addition, the Citrix and MS Active Directory Powershell module should be available for user lookup and published application creation..
.PARAMETER username
Required parameter.
User account(s) that will be assigned published application. 
.PARAMETER pubappname
Required parameter.
Common name for published application. Script will append the username to this parameter so it will be unqiue for each user. 
.PARAMETER applocation
Required parameter.
Location of executable. Used for location in published application.
.PARAMETER cmmdline
Optional parameter.
Location of file or optional parameter that could go into command line field for a published application.
.PARAMETER DeliveryGroup
Required parameter.
Which Delivery Group to publish application to.
.PARAMETER DeliveryController
Required parameter.
Which Citrix Delivery Controller (farm) to publish applicaiton with
.EXAMPLE
PS C:> .\create-pubapp.ps1 -username TESTUSER -applocation "C:\app.exe" -DeliveryGroup "Production" -DeliveryController "CitrixStudio"
    
Will create a new published application (c:\app.exe) assigned to TESTUSER and published to the Production Delivery Group.
.EXAMPLE
PS C:> .\create-pubapp.ps1 -username TESTUSER -applocation "C:\app.exe" -DeliveryGroup "Production" -DeliveryController "CitrixStudio" -verbose

Will create a new published application (c:\app.exe) assigned to TESTUSER and published to the Production Delivery Group.
Feedback and progress messages will be displayed.
.INPUTS
None. You cannot pipe objects to this script.
.OUTPUTS
No objects are output from this script.
This script creates a Xendesktop 7.x published application.
.NOTES
NAME: create-pubapp.ps1
VERSION: 2.00
CHANGE LOG - Version - When - What - Who
1.00 - 07/1/2015 - Initial script - Alain Assaf
1.01 - 07/6/2015 - Modified module import fuctions to use -like instead of -eq Created new parameter to capture delivery controller
2.00 - 07/6/2015 - Major edit to make this script more generic
AUTHOR: Alain Assaf
LASTEDIT: July 6, 2015
.LINK
http://www.linkedin.com/in/alainassaf/
http://wagthereal.com
http://www.icenlemon.co.uk/blog/?p=307
http://stackoverflow.com/questions/8388650/powershell-how-can-i-stop-errors-from-being-displayed-in-a-script

Param(
	[parameter(Position = 0, Mandatory=$True )]
	[ValidateNotNullOrEmpty()]
	$username,

	[parameter(Position = 1, Mandatory=$True )]
	[ValidateNotNullOrEmpty()]
	$pubappname,

	[parameter(Position = 2, Mandatory=$True )]
	[ValidateNotNullOrEmpty()]
	$applocation,

	[parameter(Position = 3, Mandatory=$False )]
	[ValidateNotNullOrEmpty()]
	$cmmdline,

	[parameter(Position = 4, Mandatory=$True )]
	[ValidateNotNullOrEmpty()]
	$DeliveryGroup,

	[parameter(Position = 5, Mandatory=$True )]
	[ValidateNotNullOrEmpty()]
	$DeliveryController
)

#Constants
$datetime = get-date -format "MM-dd-yyyy_HH-mm"
$ScriptRunner = (get-aduser $env:username | select name).name
$PSModules = ("activedirectory")
$PSSnapins = ("*citrix*")
$compname =  (gci env:COMPUTERNAME).value
#$ErrorActionPreference= &#039;silentlycontinue&#039; # Comment out to see all error messages
$addomain = (Get-ADDomain).name

### FUNCTION: get-mymodule #####################################################
Function Get-MyModule {
	Param([string]$name)
	if(-not(Get-Module -name $name)) {
		if(Get-Module -ListAvailable | Where-Object { $_.name -like $name }) {
			Import-Module -Name $name
			$true
		} #end if module available then import
		else { $false } #module not available
		} # end if not module
	else { $true } #module already loaded
}
### FUNCTION: get-mymodule #####################################################

### FUNCTION: get-mysnapin #####################################################
Function Get-MySnapin {
	Param([string]$name)
	if(-not(Get-PSSnapin -name $name)) {
		if(Get-PSSnapin -Registered | Where-Object { $_.name -like $name }) {
			add-PSSnapin -Name $name
			$true
		} #end if module available then import
		else { $false } #snapin not available
	} # end if not snapin
	else { $true } #snapin already loaded
}
### FUNCTION: get-mysnapin #####################################################

### FUNCTION: Check-ADUser #####################################################
#Function to check if user account exists in AD
Function Check-ADUser
{
	Param ($usrname)

	$isuser = $(try {Get-ADUser $usrname} catch {$null})
	if ($isuser -ne $null) {
		return $true
	} else {
		return $false
	}
}
### FUNCTION: Check-ADUser #####################################################

#Import Module(s) and Snapin(s)
foreach ($module in $PSModules) {
	if (!(get-mymodule $module)) {
		write-verbose "$module PowerShell Cmdlet not available."
		write-verbose "Please run this script from a system with the $module PowerShell Cmdlets installed."
		exit
	}
}
foreach ($snapin in $PSSnapins) {
	if (!(get-MySnapin $snapin)) {
		write-verbose "$snapin PowerShell Cmdlet not available."
		write-verbose "Please run this script from a system with the $snapin PowerShell Cmdlets installed."
		exit
	}
}

#Confirm Delivery Group Exists
$testDG = Get-BrokerDesktopGroup $DeliveryGroup -AdminAddress $DeliveryController
if ($testDG -eq $null) {
	write-verbose "Delivery group - $DeliveryGroup - DOES NOT EXIST. Exiting"
	exit
}

#Confirm applicaiton exists on machine you are running the script from. If False warn admin.
if (test-path $applocation) {
} else {
	write-verbose "Published Application Location - $applocation - is not valid on $compname"
}

foreach ($user in $username) {
	if (Check-ADUser $user) { #User Exists - proceed
		write-verbose "User -$user - exists"
		$pubname = $pubappname + "-" + $user
		$newapp = new-BrokerApplication -name $pubname -commandlineexecutable $applocation -DesktopGroup $DeliveryGroup -ApplicationType HostedOnDesktop -CommandLineArguments $cmmdline -UserFilterEnabled $true -AdminAddress $DeliveryController 
		Add-BrokerUser "$addomain\$user" -Application $newapp.Name
	} else {
		write-verbose "User - $user - DOES NOT EXIST"
	}
}
```

## Wait...what about the published application icon? ##
One of the parameters in the new-brokerapplication cmdlet is <em>IconUid</em>. If you want to assign something other than the default you can add this parameter to line 168 in the above script. I did not find an automated way to assign this icon. So, in this case I created an application using the GUI (and chose a non-default icon) and then ran this powershell command to see what the iconuid is:

```posh
get-brokerapplication -name "Notepad"
```

<pre>AdminFolderName                  :
AdminFolderUid                   : 3
ApplicationName                  : Notepad
ApplicationType                  : HostedOnDesktop
AssociatedDesktopGroupPriorities : {0}
AssociatedDesktopGroupUUIDs      : {1a9c3138-d236-4d70-a20b-78ff85d55897}
AssociatedDesktopGroupUids       : {4}
AssociatedUserFullNames          : {Domain Users}
AssociatedUserNames              : {Domain\Domain Users}
AssociatedUserUPNs               : {}
BrowserName                      : Notepad
ClientFolder                     : 
CommandLineArguments             : 
CommandLineExecutable            : C:\windows\system32\notepad.exe
CpuPriorityLevel                 : Normal
Description                      : 
Enabled                          : True
IconFromClient                   : False
IconUid                          : 2
MetadataKeys                     : {}
MetadataMap                      : {}
Name                             : Notepad
PublishedName                    : Notepad
SecureCmdLineArgumentsEnabled    : True
ShortcutAddedToDesktop           : False
ShortcutAddedToStartMenu         : False
StartMenuFolder                  : 
UUID                             : c7eb9ced-c7b4-4ea7-b58a-9157e06c144f
Uid                              : 14
UserFilterEnabled                : True
Visible                          : True
WaitForPrinterCreation           : False
WorkingDirectory                 :</pre>

Now you have the iconuid and it will apply the same icon to each published application instance.

Take care and have fun with XenDesktop 7.x

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*