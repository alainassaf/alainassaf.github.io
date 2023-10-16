---
layout: post
title: Automating User Creation
subtitle: Using PowerShell
date: 2014-06-10
readtime: true
tags: [Active Directory, Administration, PowerShell, Scripting]
---

# Intro #
I recently began a new job and it is unique for me. The organization I work for actually helps to generate revenue, and we are a customer of  corporate IT. This is a first for me where I’m not working directly for IT. I had just left a job where corporate viewed IT as too complex, too costly, and not responsive to the business. As a result, 80%-90% of the staff was fired (or re-badged) and all IT service and infrastructure was outsourced. 

As a manager, I had to preside over the knowledge transfer of my teams to the outsource company and then let  them go. At the same time, I had to maintain some productivity in a morale sapping environment. Needless it say is was rough, but many of my former team members got great jobs afterward as did I.

This outsourcing process had already occurred with my current employer. So, the business engages IT to implement projects, but in my case, they were not able to provide this particular service. So I find that I’m running an independent cloud implementation of XenDesktop. This includes AD (users, groups, OU’s, GPO’s – no trust with corporate AD), DHCP, DNS, File servers, XenApp, vDisk creation, Remote Access, Microsoft updates, and software testing/installs/upgrades. Basically everything from soup to nuts.

This project needs to grow to include many more users, but there is no automation present. So I’ve begun examining the biggest “pain” points in this environment to scale it out. First, we will look at user creation.

# AD DC #
It has been years since I’ve been saddled with user account management (except for service accounts and the like). Our cloud users have to remember a separate set of credentials to use our cloud environment; this is already a barrier to adoption. To relieve this, we have made our usernames match the corporate domain usernames, and left it up to the user to make the passwords the same. User account creation begins with the user filling out a SharePoint form. Then we start a manual process to create the user’s account. To relieve this, I exported a request to a CSV file and wrote a PowerShell script that runs in the cloud to create the user, set the password, set the user profile locations, and assign groups. I reviewed several websites for ideas and I noted them in my script’s help area.

# Assumptions #
For the purposes of this script, the CSV file has the following header fields:
<table border="0" width="400" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="143">Status</td>
<td valign="top" width="257">Records status of user request in SharePoint site.</td>
</tr>
<tr>
<td valign="top" width="143">Name</td>
<td valign="top" width="257">In the format of Lastname, Firstname</td>
</tr>
<tr>
<td valign="top" width="143">Account</td>
<td valign="top" width="257">Corporate AD username</td>
</tr>
<tr>
<td valign="top" width="143">Password</td>
<td valign="top" width="257">Corporate AD Password</td>
</tr>
<tr>
<td valign="top" width="143">Cloud Version</td>
<td valign="top" width="257">Used to assign groups</td>
</tr>
<tr>
<td valign="top" width="143">Email Address</td>
<td valign="top" width="257">Corporate email</td>
</tr>
<tr>
<td valign="top" width="143">Position/Title</td>
<td valign="top" width="257">Title – used to assign groups</td>
</tr>
<tr>
<td valign="top" width="143">Office Number</td>
<td valign="top" width="257">Work Phone</td>
</tr>
<tr>
<td valign="top" width="143">Mobile Number</td>
<td valign="top" width="257">Mobile Phone</td>
</tr>
<tr>
<td valign="top" width="143">Department Cost Center</td>
<td valign="top" width="257">Added to notes in user account</td>
</tr>
<tr>
<td valign="top" width="143">Manager</td>
<td valign="top" width="257">Added to notes in user account (we do not assume that the manager has a user account in the Cloud AD domain)</td>
</tr>
<tr>
<td valign="top" width="143">Manager Office Number</td>
<td valign="top" width="257">Added to notes in user account</td>
</tr>
<tr>
<td valign="top" width="143">Location or Region</td>
<td valign="top" width="257">Used for contact info</td>
</tr>
<tr>
<td valign="top" width="143">Modality</td>
<td valign="top" width="257">Used for Organization Info</td>
</tr>
<tr>
<td valign="top" width="143">Group</td>
<td valign="top" width="257">Used for Organization Info</td>
</tr>
</tbody>
</table>

It is assumed that you are running this script as a domain admin and the server you are running it from has the Microsoft AD PowerShell cmdlets installed.

If you wish to get feedback from the script while it is running use the -verbose parameter - i.e. >create-clouduser.ps1 -verbose

# TO DO’s #
I don't have an SMTP server setup in my cloud environment. When I do, I will add routines to the script to check a “New User” mailbox for new emails and then send confirmation to these users that their account was created. This will allow me to bridge the 2 domains (until corporate IT agrees to set up a domain trust).

# The Script #

```posh
<#
.SYNOPSIS
    Reads CSV file and creates a user in the YOURCOMPANY domain.
.DESCRIPTION
    Reads CSV file and creates a user in the YOURCOMPANY domain.  

    It is recommended that this script be run as an admin. In addition, the Microsoft Active Directory Powershell Cmdlets must be available for user creation.
.PARAMETER UserOU
    Defaults to OU=Users,DC=YOURCOMPANY,DC=local
    OU location where user account will be created. Input must match above format.
.PARAMETER UserFolder
    Defaults to any CSV file in C:\scripts\NewUserRequests Folder
    Enter a path to folder where CSV files exist. CSV files must have the following fields:
    Status
    Name
    Account
    Password
    Cloud Version
    Email Address
    Position/Title
    Office Number
    Mobile Number
    Department Cost Center
    Manager
    Manager Office Number
    Location or Region
    Modality
    Group
.EXAMPLE
    PS C:\PSScript &gt; .\create-cloudixuser.ps1
    Will use all default values.
    User OU=Users,DC=YOURCOMPANY,DC=local
    UserFolder=c:\scripts\NewUserRequests
.EXAMPLE
    PS C:\PSScript &gt; .\create-cloudixuser.ps1 -verbose
    Will use all default values.
    User OU=Users,DC=YOURCOMPANY,DC=local
    UserFolder=c:\scripts\NewUserRequests
    Will write feedback/progress messages.
.INPUTS
    None.  You cannot pipe objects to this script.
.OUTPUTS
    To see feedback messages use the -verbose common parameter. No objects are output from this script.  This script creates a user creation log.
.NOTES
    NAME: create-clouduser.ps1
    VERSION: 1.00
    CHANGE LOG - Version - When - What - Who
                 1.00 - 06/06/2014 - Initial script - Alain Assaf
    AUTHOR: Alain Assaf
    LASTEDIT: June 6, 2014
.LINK
http://www.linkedin.com/in/alainassaf
http://wagthereal.com
http://windowsitpro.com/blog/what-do-not-do-powershell-part-1
http://stackoverflow.com/questions/6828055/powershell-checking-if-ou-exist
http://blogs.metcorpconsulting.com/tech/?p=1723
http://stackoverflow.com/questions/14902501/powershell-script-with-params-and-functions
http://stackoverflow.com/questions/11526285/how-to-count-objects-in-powershell
http://stackoverflow.com/questions/5203730/cut-off-text-in-string-after-before-seperator-in-powershell
http://dxpetti.com/blog/?p=442
http://technet.microsoft.com/en-us/library/dd378958%28v=ws.10%29.aspx
http://powershell.org/wp/forums/topic/set-aduser-append-to-ad-notes-field/
http://www.out-web.net/?p=1233
http://blogs.technet.com/b/heyscriptingguy/archive/2010/07/11/hey-scripting-guy-weekend-scripter-checking-for-module-dependencies-in-windows-powershell.aspx
http://technet.microsoft.com/en-us/library/ff730937.aspx
#>

Param(
    [parameter(Position = 0, Mandatory=$False )]
    [ValidateNotNullOrEmpty()]
    [string]$UserOU="OU=Users,DC=YOURCOMPANY,DC=local",

    [parameter(Position = 1, Mandatory=$False )]
    [ValidateNotNullOrEmpty()]
    [string]$UserFolder="C:\scripts\NewUserRequests"
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

### FUNCTION: get-csvdata #####################################################
function get-csvdata($CSVFolder) {
    $DataFiles = (Get-ChildItem $CSVFolder -recurse -force | Where { $_.Name -like "*.csv" } | Foreach-Object -process { $_.FullName })
    $DataFilesCount = ($DataFiles | measure).count
    Write-Verbose "Discovered $DataFilesCount CSV Data files in $CSVFolder"  

    ForEach ($DataFilesItem in $DataFiles) {
        $FileInfo = Get-Item $DataFilesItem
        $LogDate = $FileInfo.LastWriteTime
        Write-Verbose "Reading data from $DataFilesItem ($LogDate ) "
        [array]$CSVData += Import-CSV $DataFilesItem -header status,name,Account,Password,CloudVer,Email,Title,WorkNumber,MobileNumber,CostCenter,Manager,ManagerWorkPhone,Location,Modality,Group
        ## Only use the header if you want to rename the attributes for the imported objects. If the column headers in the CSV are fine, don't use -header
    }
    [int] $CSVDataCount = $CSVData.Count
    Write-Verbose "Imported $CSVDataCount records"
    Return ($CSVData)
}
### FUNCTION: get-csvdata #####################################################  

#Import Module(s)
if (!(get-mymodule activedirectory)) {
    write-verbose "Microsoft Active Directory PowerShell Cmdlet not available."
    write-verbose "Please run this script from a system with the Microsoft Active Directory PowerShell Cmdlets installed."
    exit
}

#Constants
$datetime = get-date -format "MM-dd-yyyy_HH-mm"
$Domain="@YOURCOMPANY.local"
$CloudProfilePath = "\\PROFILESERVER.YOURCOMPANY.local\upm\"
$CloudRDSProfilePath = "\\PROFILESERVER.YOURCOMPANY.local\profiles\"
$ScriptRunner = (get-aduser $env:username | select name).name  

#Confirm OU is valid
If (!([adsi]::Exists("LDAP://$UserOU"))) {
    write-verbose "$UserOU IS NOT VALID"
    write-verbose "Please use the following format: OU=Users,DC=YOURCOMPANY,DC=local"
    Exit
}

#Confirm CSV Folder exists and has CSV files. If True get the files, otherweise exit
if (test-path $UserFolder) {
    if ((Get-ChildItem $UserFolder | where {$_.name -like "*.csv"}) -ne $null) {
        $UserList = get-csvdata($UserFolder)
    } else {
        write-verbose "No CSV files in $UserFolder."
        Exit
    }
} else {
    write-verbose "$UserFolder is not valid"
    Exit
}

#Create Users
foreach ($user in $UserList) {
    if (!([bool]([adsisearcher]"samaccountname=$user.Account").FindOne()) -and ($user.status -ne 'Status')) {
        #Get first and last name
        $pos = ($user.name).IndexOf(",")
        $SurName = ($user.name).Substring(0, $pos)
        $GivenName = ($user.name).Substring($pos+2)
        #Create other account name data
        $DisplayName = $GivenName+ " " + $SurName
        $Initials = ($GivenName).Substring(0,1) + ($SurName).Substring(0,1)
        $Username = $user.Account
        $UPN=$Username+$Domain
        #Get Cloud password to the Philips CODE password
        $Password = $user.Password
        #Create Description
        $Description = $User.Modality + " " + $user.Title
        #Create Contact Info
        $MobilePhone = $User.MobileNumber
        $OfficePhone = $User.WorkNumber
        $Email = $User.Email
        #Create Organization Info
        $Company = "YOUR COMPANY"
        $Department = $User.Modality + " " + $user.Group
        $Divsion = $User.Modality
        $EmployeeNumber = $User.Account
        $Manager = $user.Manager
        $Office = $User.Location
        $Title = $user.Title
        #Create User Profile Location
        $ProfilePath = $CloudProfilePath + $Username
        #Create User in AD
        new-aduser -name $Displayname -DisplayName $DisplayName -GivenName $GivenName -Surname $SurName -SamAccountName $UserName -UserPrincipalName $UPN -Description $Description -MobilePhone $MobilePhone -OfficePhone $OfficePhone -EmailAddress $Email -Company $Company -Department $Department -Division $Division -EmployeeNumber $EmployeeNumber -Office $Office -Title  $Title -ProfilePath $ProfilePath -Path $UserOU
        write-verbose "New AD User Account created for $DisplayName"
        #Set Password
        set-adaccountpassword -Identity $Username -NewPassword (ConvertTo-SecureString -AsPlainText $Password -Force)
        #Set Remote Desktop Services User Profile
        $RDSProfilePath = $CloudRDSProfilePath + $username
        Get-ADUser $username | ForEach-object {
            $ADSI = [ADSI]('LDAP://{0}' -f $_.DistinguishedName)
            try {
                $ADSI.InvokeSet('TerminalServicesProfilePath',$RDSProfilePath)
                $ADSI.SetInfo()
            } catch { Write-Verbose $Error[0] }
        }
        #Add User to groups
        #Based on Cloud Version - modify as needed to match AD groups
        $GrpArray = @()
        switch ($user.cloudver) {
            "Field Service" {}
            "Work from Home" {$GrpArray += "WFH"}
            "Pilot" {}
            "QA" {}
            "Sales" {}
            "Inventory" {}
        }
        #Based on Title - modify as needed to match AD groups
        switch ($user.title) {
            "Admin Assistant" {}
            "Director" {$GrpArray += "Leadership"}
            "FEP" {$GrpArray += ""}
            "Helpdesk" {$GrpArray += "Helpdesk"}
            "Helpdesk LEAD" {$GrpArray += "Domain Admins"; $GrpArray += "Helpdesk"}
            "FSE" {$GrpArray += "FSE"}
            "Pre-Sales" {$GrpArray += "Sales"}
            "RF Engineer" {$GrpArray += "Some-Application-Users" ; $GrpArray += "RF"}
            "RLM" {}
            "Sales" {$GrpArray += "Sales"}
            "SDC" {$GrpArray += "SDC"}
            "TC" {$GrpArray += "TC"}
            "Other" {}
        }
        Add-ADPrincipalGroupMembership $username -MemberOf $GrpArray
        write-verbose "The following groups were added to $DisplayName:"
        write-verbose "$GrpArray"
        #Add Notes to user
        get-aduser $username -properties info | foreach { Set-ADUser -Identity $_.samaccountname -Replace @{info="$($_.info)Manager - $Manager"} }
        get-aduser $username -properties info | foreach { Set-ADUser -Identity $_.samaccountname -Replace @{info="$($_.info)`r`nManager Phone - $($user.ManagerWorkPhone)"} }
        get-aduser $username -properties info | foreach { Set-ADUser -Identity $_.samaccountname -Replace @{info="$($_.info)`r`nEmployee Cost Center - $($user.costcenter)"} }
        get-aduser $username -properties info | foreach { Set-ADUser -Identity $_.samaccountname -Replace @{info="$($_.info)`r`nAccount created on: $datetime by $ScriptRunner"} }
        #Enable Account
        Enable-ADAccount -Identity $Username
    }
}
```

### Value for Value ###
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*