---
layout: post
title: Display Maintenance Message for a XenApp Published Application
date: 2014-08-19
readtime: true
tags: [Administration, PowerShell, Scripting, XenApp]
thumbnail-img: /assets/img/display-maintenance-message-for-a-xenapp-published-application/wherearemycitrixapps.png
share-img: /assets/img/display-maintenance-message-for-a-xenapp-published-application/wherearemycitrixapps.png
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/wherearemycitrixapps.png" 
    alt="motherofdragons">

# Intro #
For years, management of Citrix Published Applications has been pretty heavy-handed. You had the following options:
<ol>
	<li>You had enough servers to ensure your users always had access to their apps.</li>
	<li>You established a regular maintenance period that everyone knew about (I’m trying not to burst out laughing) that allowed you to perform application and server maintenance.</li>
	<li>You implemented an application virtualization technology that allowed for incremental testing and roll-out of application changes without affecting all your users.</li>
	<li>You used the Citrix console to disable and hide an application (resulting in calls to the help desk like the above picture).</li>
</ol>

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/image.png" 
    alt="image">

In this blog post, I’ll present a PowerShell script that allows you to display a maintenance message instead of hiding the application. This allows you to do maintenance and give users some feedback about what’s going on.

## Disabling the application ##
We’re assuming the runner of the script has XenApp farm admin permissions enough to change published application properties and that the XenApp 6.5 SDK is installed.  When the *change-appmaintenance.ps1* script runs, it creates a text file with the same name as the published app under `C:\_scripts\appmaintenance` that stores 3 items of information (this assumes the `–disabled` flag is used):
<ol>
	<li>The original Command Line field of the published application (used when we need to put the application back in production.</li>
	<li>The person who ran the script.</li>
	<li>The current date and time (when the script was run).</li>
</ol>
Here’s a command line example (the –verbose flag is optional, but shows feedback messages):

```posh
PS C:\_scripts> .\change-appmaintenance.ps1 -PublishedApp "testdisable" -disable -Verbose
```

Here is the maintenance file created for the TestDisable published application.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/image1.png" 
    alt="image1">

Once the information is saved, the script changes the command line to the following:

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/image2.png" 
    alt="image2">

The show-appmaintenancebox.ps1 script is simple, but you can add more information as needed. You could further customize the maintenance message by passing parameters, but you may run into the character limit (256 characters) in the Command Line field. In this case you will get the following message:

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/image3.png" 
    alt="image3">

You could also solve the above issue by calling a batch file to run the script.

```posh
$wshell = New-Object -ComObject Wscript.Shell
$wshell.Popup("This application is unavailable because it is undergoing maintenance. Please try again later.",0,"APPLICAITON UNDER MAINTENANCE",0)
```

> NOTE: More info about the Popup Method is [here](http://msdn.microsoft.com/en-us/library/x83z1d9f%28v=vs.84%29.aspx)

When a user launches the application, the script now displays this window.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/image4.png" 
    alt="image4">

## Enabling the Application ##
To reverse the changes you run the same command without the –Disabled flag (the –verbose flag is optional, but shows feedback messages):

```posh
PS C:\_scripts> .\change-appmaintenance.ps1 -PublishedApp "testdisable" –Verbose
```

The script will check for the published application maintenance file under C:\_scripts\appmaintenance, read the file, and use the first line to change the Command Line back to the correct one for the application. Then it will delete the maintenance file.

# The Script #

```posh
<#
.SYNOPSIS
	Disables (or re-enables) a published application by replacing the original command line with one that
 calls a pop-up window.
.DESCRIPTION
	Disables (or re-enables) a published application by replacing the original command line with one that
 calls a pop-up window. 

	It is recommended that this script be run as an a XenApp Farm administrator.
 This script assumes that the XenApp 6.5 powershell commandets are installed.
.PARAMETER PublishedApp
    Required parameter. Application that will be enabled or disabled.
.PARAMETER Disable
    Conditional parameter.
    If present, the application is disabled and maintenance message will be displayed. If not, then the maintenance message
    is removed and applicaiton is republished.
.EXAMPLE
	PS C:\PSScript > .\change-appmaintenance.ps1 -PublishedApp $PublishedApplicationName

	Script will try and re-enable the application.
 Will not display feedback
.EXAMPLE
	PS C:\PSScript > .\change-appmaintenance.ps1 -PublishedApp $PublishedApplicationName -verbose

	Script will try and re-enable the application.
 Will display feedback
.EXAMPLE
	PS C:\PSScript > .\change-appmaintenance.ps1 -PublishedApp $PublishedApplicationName -Disabled -verbose

	Script will disable the application.
 Will display feedback
.INPUTS
	PublishedApp maintenance file. If the file exists it assumes that the application was already put in maintenance. The script will read in the
    information from the file and take it out of maintenance (assuming the disabled flag is not present). It will delete the existing file if
    taking the application is taken out of maintenance.
.OUTPUTS
	A maintenance file will be generated per application (assuming the disabled switch is present). The file will contain the original location of the application from the published application
    properties, the name of the script runner, and the date the change was made.
.NOTES
	NAME: change-appmaintenance.ps1
	VERSION: 1.00
    CHANGE LOG - Version - When - What - Who
                 1.00 - 08/15/2014 - Inititail script - Alain Assaf
	AUTHOR: Alain Assaf
	LASTEDIT: August 15, 2014
.LINK
 http://www.linkedin.com/in/alainassaf/
 http://wagthereal.com
 http://gallery.technet.microsoft.com/scriptcenter/PowerShell-Message-Box-6c6e4f75
#>

Param(
    [parameter(Position = 0, Mandatory=$true )]
    [ValidateNotNullOrEmpty()]
	[string]$PublishedApp,

    [parameter(Position = 1, Mandatory=$False )]
    [ValidateNotNullOrEmpty()]
    [switch]$Disable)

### FUNCTION: get-mysnapin ####################################################
Function Get-MySnapin {
    Param([string]$name)
    if(-not(Get-PSSnapin -name $name)) {
        if(Get-PSSnapin -Registered | Where-Object { $_.name -eq $name }) {
            add-PSSnapin -Name $name
            $true
        } #end if module available then import
        else { $false } #snapin not available
        } # end if not snapin
    else { $true } #snapin already loaded
}
### FUNCTION: get-mysnapin ####################################################

#Constants
$datetime = get-date -format "MM-dd-yyyy_HH-mm"
$Domain=(Get-WmiObject Win32_ComputerSystem).Domain
$ScriptRunner = (get-aduser $env:username | select name).name
$XAZDC = "XenApp65ZDC.local"
$appmaintfolder = "C:\_scripts\appmaintenance"
$PSSnapins = ("Citrix.XenApp.Commands")
$XApps = New-Object System.Collections.ArrayList

#Populate XApps
foreach ($a in $PublishedApp) {$XApps.Add($a)>$null}

#Import Snapin(s)
foreach ($snapin in $PSSnapins) {
    if (!(get-MySnapin $snapin)) {
        write-verbose "$snapin PowerShell Cmdlet not available."
        write-verbose "Please run this script from a system with the $snapin PowerShell Cmdlets installed."
        exit
    }
}

#Confirm application(s) exists
foreach ($apps in $XApps) {
    $appresult = Get-XAApplication -ComputerName $XAZDC -BrowserName $apps -ErrorAction SilentlyContinue
    if ([boolean]($appresult)) {
        write-verbose "$apps exists"
        if ($Disable) {   #Disable flag is present
            $appmaintfile = $apps + ".txt"
            if (test-path -Path $appmaintfolder\$appmaintfile) {
                write-verbose "$appmaintfile already exists. Please confirm application name. No changes made"
            } else {      #Create maintenance file and replace app's command line
                add-content -Value $appresult.CommandLineExecutable -Path $appmaintfolder\$appmaintfile
                Add-Content -Value $ScriptRunner -Path $appmaintfolder\$appmaintfile
                Add-Content -Value $datetime -Path $appmaintfolder\$appmaintfile
                Set-XAApplication -computername $XAZDC -BrowserName $apps -CommandLineExecutable "powershell.exe -noprofile -executionpolicy bypass -command c:\_scripts\Show-appmaintenancebox.ps1"
            }
        } else {          #Read maintenance file and restore app's command line
            $appmaintfile = $apps + ".txt"
            if (test-path -Path $appmaintfolder\$appmaintfile) {
                $fileinfo = get-content -path $appmaintfolder\$appmaintfile
                Set-XAApplication -computername $XAZDC -BrowserName $apps -CommandLineExecutable $fileinfo[0]
                remove-item -Path .\appmaintenance\$appmaintfile
                Write-Verbose "Application maintenance removed for $apps"
            } else {
                write-verbose "*** ERROR *** expected maintenance file:$appmaintfile is missing. Please confirm application name. Script will exit"
                exit
            }
        }
    } else {
        Write-Verbose "$apps does not exist"
    }
}
```

## Script Logic Flow ##
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/display-maintenance-message-for-a-xenapp-published-application/changeappmaintenance_scriptflow.png" 
    alt="changeappmaintenance_scriptflow">

## Future tasks ##
To use this with your users, I would recommend that you try calling the PowerShell script with VBS. This should allow you to hide the PowerShell window that calls the script. You can read more here: <a href="http://stackoverflow.com/questions/14016488/hiding-a-powershell-window-using-vbscript-whitespace-in-the-file-filepath">Hiding a Powershell window using VBscript</a>

I hope you found this script useful and that it may help you. I welcome all comments and suggestions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*