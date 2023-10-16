---
layout: post
title: The Ultimate User Deletion Script?
subtile: Using PowerShell
date: 2014-07-25
readtime: true
tags: [Active Directory, Administration, PowerShell, Scripting, XenApp, XenDesktop]
thumbnail-img: /assets/img/the-ultimate-user-deletion-script/akkyu.jpg
share-img: /assets/img/the-ultimate-user-deletion-script/akkyu.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/the-ultimate-user-deletion-script/akkyu.jpg" 
    alt="yeah">

# Intro #
I’ve spent the better part of 4 days working on a monster script. I needed this script to perform the following actions:
<ol>
	<li>Take a list of usernames and ensure they exist in Active Directory (AD).</li>
	<li>Take a list of usernames and disable their AD accounts</li>
	<li>Take a list of usernames and delete their AD accounts</li>
	<li>Take a list of usernames and delete any files in their User profile and RDS profile</li>
	<li>Take a list of usernames and delete any local profiles present on XenApp Servers</li>
	<li>Take a list of usernames and delete any of their assigned XenDesktops</li>
	<li>Take a list of usernames and remove their account from any published application</li>
	<li>Review this list of usernames and remove any accounts that should not be removed</li>
	<li>Provide feedback with write-verbose messages and create a log file of any actions</li>
</ol>

Needless to say, this script can easily be broken down into different functions for reusability, but I wanted an all-in-one script that would be used by other support team members. This script assumes PowerShell 2.0 and that the following cmdlets are available:
<ul>
	<li>Microsoft Active Directory</li>
	<li>Citrix XenApp Commands (SDK for XenApp 6 or 6.5 so you can run remote commands)</li>
	<li>Citrix XenDesktop Commands</li>
</ul>

The user who runs the script should be a AD domain , XenApp and XenDesktop admin.  I do not recommend using this script as is, but you may find parts you can use in your environment.  Also review the URL’s in the .LINK section. I used ideas and code from these web pages to help write this script.

You can edit the #CONSTANTS section for your environment.

# The Script #

```posh
<#
.SYNOPSIS
	Takes ad username or object of usernames and deletes user's resources and account from the domain and Citrix environment.
.DESCRIPTION
	Removes a user's account and resources from the AD domain and Citrix environment.

	It is recommended that this script be run as an admin. In addition, the Microsoft Active Directory, XenDesktop, XenApp Powershell Cmdlets must be available for user and desktop deletion.
.PARAMETER username
      Required parameter.
      User account(s) that will be deleted. User accounts must be disabled before deletion. See disable parameter.
.PARAMETER disable
    Optional switch parameter.
    Defaults to $false.
    If present, user accounts will just be disabled.
.EXAMPLE
	PS C:\PSScript > .\delete-citrixuser.ps1 -username "someuser"

	Will use all default values.
    No feedback messages will be shown.
    All user accounts are expected to be already disabled otherwise, no accounts will be deleted.
.EXAMPLE
	PS C:\PSScript > .\delete-citrixuser.ps1 -username "someuser" -verbose

	Will use all default values.
    Feedback/progress messages will be shown.
    All user accounts are expected to be disabled otherwise, no accounts will be deleted.
.EXAMPLE
    PS C:\PSScript > .\delete-citrixuser.ps1 -username "someuser" -disable -verbose

    Will use all default values.
	Will set AD user accounts to disabled.
    Feedback/progress messages will be shown.
.INPUTS
	Username or object of usernames.
.OUTPUTS
	To see feedback messages use the -verbose common parameter. No objects are output from this script.  This script creates a user deletion log.
.NOTES
	NAME: delete-citrixuser.ps1
	VERSION: 1.00
    CHANGE LOG - Version - When - What - Who
                 1.00 - 07/21/2014 - Initial script - Alain Assaf
	AUTHOR: Alain Assaf
	LASTEDIT: July 21, 2014
.LINK
    http://www.linkedin.com/in/alainassaf/
    http://wagthereal.com
    http://stackoverflow.com/questions/11605893/checking-for-the-existence-of-an-ad-object-how-do-i-avoid-an-ugly-error-message
    http://powershell.com/cs/blogs/tips/archive/2009/06/26/using-switch-parameters.aspx
    http://technet.microsoft.com/en-us/library/ee692802.aspx
    http://explorepowershell.com/2012/12/24/checking-setting-remote-desktop-services-profile-settings/
    http://winpowershell.blogspot.com/2006/08/suppressing-output-using-out-null-and.html
    http://blogs.msdn.com/b/powershell/archive/2009/12/29/arguments-for-remote-commands.aspx
    http://techibee.com/powershell/powershell-script-to-delete-windows-user-profiles-on-windows-7windows-2008-r2/1556
    http://stackoverflow.com/questions/12727388/wildcard-with-variable-in-get-aduser
    http://ss64.com/ps/do.html
    http://technet.microsoft.com/en-us/library/ee177002.aspx
    http://technet.microsoft.com/en-us/library/ee176955.aspx
    http://support.citrix.com/static/kc/CTX127254/help/
    http://social.technet.microsoft.com/Forums/windowsserver/en-US/79f9be3c-2945-471d-8a60-a1390f376d6e/removeaduser-has-no-force-option?forum=winserverpowershell
    http://titlerequired.com/2012/10/29/powershell-make-it-do-something-useful/
#>

Param(
    [parameter(Position = 0, Mandatory=$True )]
    [ValidateNotNullOrEmpty()]
	$username,

    [parameter(Position = 1, Mandatory=$False )]
    [ValidateNotNullOrEmpty()]
    [switch]$disable
	)

### FUNCTION: get-mymodule #####################################################
Function Get-MyModule {
    Param([string]$name)
    if(-not(Get-Module -name $name)) {
        if(Get-Module -ListAvailable | Where-Object { $_.name -eq $name }) {
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
        if(Get-PSSnapin -Registered | Where-Object { $_.name -eq $name }) {
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

### FUNCTION: Remove-UserProfile################################################
Function Remove-UserProfile
{
    param(
        [parameter(ValueFromPipeline=$true,ValueFromPipelineByPropertyName=$true)]
        [ValidateNotNullOrEmpty()]
        [string[]]$ComputerName = $env:computername,            

        [parameter(mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$UserName, 

        [parameter(mandatory=$false)]
        [ValidateNotNullOrEmpty()]
        [string]$localpath 

    )            

    foreach($Computer in $ComputerName) {
        Write-Verbose "Looking for local profiles on $Computer"
        if(Test-Connection -ComputerName $Computer -Count 1 -ea 0) {
            $UserProfile = Get-WmiObject Win32_UserProfile -Computer $Computer -ea 0 -filter “localpath='$localpath'”
            if (!$UserProfile) {
                write-Verbose “$Username not found on $Computer”
            } else {
                $UserProfile | Remove-WmiObject
                Write-Verbose "$UserName profile deleted successfully on $Computer"
            }
        }  else {
            write-verbose "Cannot connect to $Computer"
        }
    }
}
### FUNCTION: Remove-UserProfile################################################

#Constants
$datetime = get-date -format "MM-dd-yyyy_HH-mm"
$Domain=(Get-WmiObject Win32_ComputerSystem).Domain
$UserOU="OU=Users,DC=domain,DC=local"
$CloudProfilePath = "\\userprofileserver.domain.local\upm\"
$CloudRDSProfilePath = "\\userprofileserver.domain.local\profiles\"
$ScriptRunner = (get-aduser $env:username | select name).name
$XAZDC = "XenAppZDC.domain.local"
$PSModules = ("activedirectory")
$PSSnapins = (
    "Citrix.ADIdentity.Admin.V1",
    "Citrix.Broker.Admin.V1",
    "Citrix.Common.Commands",
    "Citrix.Common.GroupPolicy",
    "Citrix.Configuration.Admin.V1",
    "Citrix.Host.Admin.V1",
    "Citrix.LicensingConfig.Admin.V1",
    "Citrix.MachineCreation.Admin.V1",
    "Citrix.MachineIdentity.Admin.V1",
    "Citrix.XenApp.Commands")
$accountnames = New-Object System.Collections.ArrayList
#Any account in DONOTDELETE array will always be removed from accountnames array and will not be disabled or deleted.
$DONOTDELETE = New-Object System.Collections.ArrayList
$DONOTDELETE.Add("Guest")>$null
$DONOTDELETE.Add("krbtgt")>$null

#Get List of XenApp Servers
$XAServers = get-xaserver -ComputerName $XAZDC | select servername

#Populate accountnames
foreach ($a in $username) {$accountnames.Add($a)>$null}

#Remove any users in accountnames who are in the DONOTDELETE array
foreach ($b in $DONOTDELETE) {$accountnames.remove($b)>$null}

#initalize output array
$finalout = @()

#Import Module(s) and Snapin(s)
foreach ($module in $PSModules) {
    if (!(get-mymodule $module)) {
        write-verbose "$module PowerShell Cmdlet n ot available."
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

#Confirm OU is valid
If (!([adsi]::Exists("LDAP://$UserOU"))) {
    write-verbose "$UserOU IS NOT VALID"
    write-verbose "Please use the following format: OU=Users,DC=domain,DC=local"
    Exit
}

#Confirm user(s) exists
foreach  ($usr in $accountnames) {
    $UserStatus = Check-ADUser $usr
    If ($UserStatus) { #User Exists - delete or disable user
        write-verbose "USER EXISTS: $usr"
        If ($disable) { #Disable is true - just disable the user
            write-verbose "DISABLING: $usr"
            disable-ADAccount -Identity $usr
            get-aduser $usr -properties info | foreach { Set-ADUser -Identity $_.samaccountname -Replace @{info="$($_.info)`r`nAccount disabled on: $datetime by $ScriptRunner"} }
            $finalout += "$usr disabled by $ScriptRunner on $datetime"
        } Else {        #Disable flag is false - delete user (if already disabled)
            #Confirm user account is disabled
            $citrixuser = Get-ADUser $usr -properties profilepath | select -ExcludeProperty disting*
            if ($citrixuser.Enabled) { #User account is not disabled - write error and go to next user
                write-verbose "$usr is not disabled. Please disable prior to deletion."
                $finalout += "$usr is not disabled. Please disable prior to deletion."
            } else { #user account is disabled clean up and delete user account.
                #Get User's profile and RDS profile path
                $userprofilepath = $citrixuser.profilepath
                $citrixuser = [ADSI]“LDAP://$citrixuser”
                $rdsprofilepath = $citrixuser.psbase.InvokeGet(“terminalservicesprofilepath”)
                #Delete user's profile and RDS profile
                if (test-path $userprofilepath) {
                    remove-item -Recurse -Force $userprofilepath
                    write-verbose "FOLDER DELETED: $userprofilepath"
                    $finalout += "FOLDER DELETED: $userprofilepath"
                } elseif (test-path ($userprofilepath + ".V2")) {
                    remove-item -Recurse -Force ($userprofilepath + ".V2")
                    write-verbose ("FOLDER DELETED: $userprofilepath" + ".V2")
                    $finalout += ("FOLDER DELETED: $userprofilepath" + ".V2")
                } else {
                    write-verbose ("FOLDER DOES NOT EXIST: $userprofilepath or $userprofilepath" + ".V2")
                }
                if (test-path $rdsprofilepath) {
                    remove-item -Recurse -Force $rdsprofilepath
                    write-verbose "FOLDER DELETED: $rdsprofilepath"
                    $finalout += "FOLDER DELETED: $rdsprofilepath"
                } elseif (test-path ($rdsprofilepath + ".V2")) {
                    remove-item -Recurse -Force ($rdsprofilepath + ".V2")
                    write-verbose ("FOLDER DELETED: $rdsprofilepath" + ".V2")
                    $finalout += ("FOLDER DELETED: $rdsprofilepath" + ".V2")
                } else {
                    write-verbose ("FOLDER DOES NOT EXIST: $rdsprofilepath or $rdsprofilepath" + ".V2")
                }
                #Delete user's local profile on XenApp servers
                foreach ($srv in $XAServers) {
                    $tmpdir = invoke-command -computername $srv.servername {"$env:homedrive\users"}
                    $usrdir = $tmpdir + "\" + $usr
                    remove-userprofile -computername $srv.servername -username $usr -localpath $usrdir.Replace("\","\\") -verbose
                }
                #Delete any assigned XenDesktops
                $user = $Domain.split(".")[0] + "\" + $usr
                $UserDesktops = Get-BrokerDesktop -AssociatedUserName $user -DesktopKind "Private"
                if ($UserDesktops -ne $null) {
                    foreach ($d in $UserDesktops) {
                        set-brokerprivatedesktop $d.machinename -InMaintenanceMode $true
                        do {
                            start-sleep -s 5
                            $result1 = Get-BrokerPrivateDesktop -MachineName $d.Machinename
                        } until ($result1.InMaintenanceMode -eq $true)
                        write-verbose "$d.machinename - maintenance mode enabled"
                        New-BrokerHostingPowerAction -Action 'Shutdown' -MachineName $d.machinename
                        do {
                            start-sleep -s 5
                            $result2 = Get-BrokerHostingPowerAction -MachineName $d.MachineName | select -last 1
                        } until ($result2.State -eq "Completed")
                        write-verbose "$d.machinename - powered down"
                    }
                    #Unassign user from desktop
                    Remove-BrokerUser -Name $user -Machine $d.machinename
                    #Remove desktop from DesktopGroup
                    remove-Brokermachine -Machine $d.machinename -desktopgroup $d.desktopgroupuid
                    #Delete desktop and AD Computer Account
                    Remove-BrokerMachine -MachineName $d.machinename
                    Remove-AcctADAccount -IdentityPoolName $d.Catalogname -ADAccountName $d.machinename -RemovalOption 'Delete'
                    write-verbose "$d.machinename deleted from Desktop Studio and Active Directory"
                    $finalout += "$d.machinename deleted from Desktop Studio and Active Directory"
                } else {
                    write-verbose "No desktops associated with $user"
                    $finalout += "No desktops associated with $user"
                }
                #Remove user from any Citrix published resources
                $apps = (Get-XAApplicationReport -ComputerName $XAZDC -BrowserName * | where {$_.Accounts -contains $user})
                if ([bool]($apps -ne $null)) { #Found useraccount assigned to some published resources
                    foreach ($app in $apps) {
                        Remove-XAApplicationAccount -ComputerName $xazdc -BrowserName $app.Browsername -Accounts $user
                    }
                } else {
                    write-verbose "$usr is not assigned to any published applications"
                }
                #Delete user account from AD
                remove-aduser -identity $usr -confirm:$false
                write-verbose "$user removed from $domain"
                $finalout += "$user removed from $domain"
            }
        }
    } Else { #User does not exist - stop and go to next user
        write-verbose "$usr does not exist."
    }
}

#CreateReport
if ($finalout -ne $null) {
    $LogFileFolder = "c:\_scripts"
#    $datetime = get-date -format "MM-dd-yyyy_HH-mm"
    $LogFileName = "Citrix_DeleteUser_Report" + $datetime + ".txt"
    $LogFile = $LogFileFolder + "\" + $LogFilename
    $finalout | ft -auto | out-file $Logfile -append
}
```

I look forward to any and all comments. I’m sure there are better ways to write some of the above. There’s always more than one way to skin a cat in PowerShell.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*