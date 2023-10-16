---
layout: post
title: Message of the Day
subtitle: PowerShell
date: 2011-09-27
tags: [Administration, PowerShell, Scripting, XenApp]
cover-img: ["/assets/img/powershell-message-of-the-day/sign.jpg" : "Pixabay"]
thumbnail-img: /assets/img/powershell-message-of-the-day/sign.jpg
share-img: /assets/img/powershell-message-of-the-day/sign.jpg
---
<strong>Intro</strong>

In an effort to be more communicative with our users on changes made to our XenApp servers we decided to create a message of the day.  Here are the criteria we decided on:
<ol>
	<li>The message would show as part of the user’s startup script when logging into XenApp/XenDesktop</li>
	<li>The user would have to click a button to remove the message box and the result could be acted on by the script. In one example, if the user pressed the Yes button, then we could open a page to our intranet.</li>
	<li>A hidden flag file would be written to a user’s home directory to note that the user had already seen the message.</li>
	<li>The Message file itself should be easily edited by any member of the team and not require any scripting knowledge.</li>
</ol>
I wore Google out looking for various solutions and examples to get the script working the way I wanted. I do not consider myself an expert in PowerShell by any means, but hopefully this post will give you some ideas to try in your environment.

<strong>Assumptions</strong>

Our environment supports development, staging and production environments along with different XenApp images depending on software being delivered. As a result, the scripts and the text file that is the message of the day are sync’d to the C: drive of the provisioned image. This allows us to tailor messages to different user populations.

While running, the script will archive the existing message, and since the user does not have permission to write to the C: drive (this script runs during a user’s login as the user) I’m using an admin account to write the archive file to the C: drive.

<strong>The Script</strong>

I’m using <strong>Show-Messagebox.ps1</strong> to create the message box provided by Daniel Sörlöv (original <a href="https://the.powershell.zone/2010/12/07/show-messagebox-ps1/"><b>link</b></a>, his <a href="https://the.powershell.zone/"><b>new site</b></a>):

This script is located in the LocalCache folder along with motd.ps1 on the XenApp server.

```posh
param ($title,$text,$buttons="OK",$icon="None")

$FormsAssembly = [System.Reflection.Assembly]::LoadWithPartialName("System.Windows.Forms")

$dialogButtons = @{
 "OK"=[Windows.Forms.MessageBoxButtons]::OK;
 "OKCancel"=[Windows.Forms.MessageBoxButtons]::OKCancel;
 "AbortRetryIgnore"=[Windows.Forms.MessageBoxButtons]::AbortRetryIgnore;
 "YesNoCancel"=[Windows.Forms.MessageBoxButtons]::YesNoCancel;
 "YesNo"=[Windows.Forms.MessageBoxButtons]::YesNo;
 "RetryCancel"=[Windows.Forms.MessageBoxButtons]::RetryCancel }

$dialogIcons = @{
 "None"=[Windows.Forms.MessageBoxIcon]::None
 "Hand"=[Windows.Forms.MessageBoxIcon]::Hand
 "Question"=[Windows.Forms.MessageBoxIcon]::Question
 "Exclamation"=[Windows.Forms.MessageBoxIcon]::Exclamation
 "Asterisk"=[Windows.Forms.MessageBoxIcon]::Asterisk
 "Stop"=[Windows.Forms.MessageBoxIcon]::Stop
 "Error"=[Windows.Forms.MessageBoxIcon]::Error
 "Warning"=[Windows.Forms.MessageBoxIcon]::Warning
 "Information"=[Windows.Forms.MessageBoxIcon]::Information
}

$dialogResponses = @{
 [System.Windows.Forms.DialogResult]::None="None";
 [System.Windows.Forms.DialogResult]::OK="Ok";
 [System.Windows.Forms.DialogResult]::Cancel="Cancel";
 [System.Windows.Forms.DialogResult]::Abort="Abort";
 [System.Windows.Forms.DialogResult]::Retry="Retry";
 [System.Windows.Forms.DialogResult]::Ignore="Ignore";
 [System.Windows.Forms.DialogResult]::Yes="Yes";
 [System.Windows.Forms.DialogResult]::No="No"
}

return $dialogResponses[[Windows.Forms.MessageBox]::Show($text,$title,$dialogButtons[$buttons],$dialogIcons[$icon])]
```

The <b>Get-Checksum function</b> is from <a href="http://web.archive.org/web/20110916050221/http://www.techmumbojumblog.com/?author=2"><b>Moe Winnett</b></a> at the <a href="http://web.archive.org/web/20110917063939/http://www.techmumbojumblog.com/"><b>TechMumboJumblog</b></a>. I use this function to generate a checksum of the current message.txt file that will be displayed to the user. Once the message is displayed, a hidden .flag file is written to the user's home directory containing the message.txt checksum. When the script checks to see if the user has seen the message, a checksum is generated of the current message.txt and compared to the checksum written to the .flag file. If they match, the script assumes that the user has already seen the message and will not display it.

```posh
### Get-Checksum function #############################################

function Get-Checksum($file, $crypto_provider) {

    if ($crypto_provider -eq $null) {
        $crypto_provider = New-Object 'System.Security.Cryptography.MD5CryptoServiceProvider'
    }

    $file_info = Get-Item $file;
    trap { continue } $stream = $file_info.OpenRead()
    if ($? -eq $false) { return $null }

    $bytes = $crypto_provider.ComputeHash($stream)
    $checksum = ''
    foreach ($byte in $bytes) {
        $checksum += $byte.ToString('x2')
    }

    $stream.close() | Out-Null

    return $checksum
}

### Main Code #########################################################

##Set env variables
$hostname = Get-Content env:computername
$lc = (Get-Item env:localcache).value
$today = Get-Date
$usrname = [Environment]::UserName
$usrprofile = (Get-Item env:userprofile).value
$usrPath = "$usrprofile\$usrname"

##Set credentials
$username = "DOMAIN\CITRIXADMIN"
$Passwd = "PASSWORD1"

## Set file paths
$MsgBox = "$lc\scripts\Show-MessageBox.ps1"
$MsgPath = "$lc\message"
$MsgFile = "message.txt"
$MsgFilePath = $MsgPath + '\' + $MsgFile
$userhome = (Get-Item env:USERHOME).Value
$debug = $true

##Check if there is a message file and get its checksum, if not exit the script
if (!(Test-Path $MsgFilePath)) {
    if ($debug) {
        Write-Host "No MOTD file"
    }
    exit
} else {
    ##Check whether Message has been archived
    if (Get-Content -LiteralPath $MsgFilePath | Select-String -Pattern "ARCHIVE" -Quiet) {
        if ($debug) {
            Write-Host "MOTD file was already archived"
        }
    } else {
        if (!(Test-Path $MsgPath\archive)) {
            mkdir $MsgPath\archive | Out-Null
            if ($debug) {
                Write-Host "Created archive folder"
            }
        }
        $ArchiveNote = "`r`nARCHIVE - Delete this line if editing file"
        net use \\$Hostname\c$ /user:$username $Passwd
        $tmppath = "\\$hostname\c$\localcache\message\message.txt"
        Add-Content $tmppath $ArchiveNote
        net use \\$Hostname\c$ /delete
        Copy-Item $MsgPath\$MsgFile "$MsgPath\archive\MOTD_$((Get-Date).toString('yyyyMMdd')).txt"
        if ($debug) {
            Write-Host "Archived $MsgFilePath to $MsgPath\archive\MOTD_$((Get-Date).toString('yyyyMMdd')).txt"
        }
    }
    $MsgChecksum = get-checksum($MsgFilePath)
}

##Check if the user has already seen the message
if (Test-Path $flexpath\.flag) {
    if ($debug) { Write-Host "User has seen a message - check if it's current message" }
    $a = Get-Item $flexpath\.flag -Force
    $flgchecksum = Get-Content $a
    if ($flgchecksum -eq $MsgChecksum) {
        if ($debug) {
            Write-Host "User has seen today's message - exit"
            exit
        }
    } else {
        Remove-Item $a -Force
        $flagfile = New-Item $flexpath\.flag -type file
        Add-Content $flagfile $MsgChecksum
        Set-ItemProperty -Path $flagfile -Name Attributes -Value ([System.IO.FileAttributes]::Hidden)
        if ($debug) { Write-Host "User's old .flag file deleted" }
        if ($debug) { Write-Host "Created hidden flag file: $flagfile and wrote checksum: $MsgChecksum" }
    }
} else {
    $flagfile = New-Item $flexpath\.flag -type file
    Add-Content $flagfile $MsgChecksum
    Set-ItemProperty -Path $flagfile -Name Attributes -Value ([System.IO.FileAttributes]::Hidden)
    if ($debug) { Write-Host "User has not seen the message" }
    if ($debug) { Write-Host "Created hidden flag file: $flagfile and wrote checksum: $MsgChecksum" }
}

## Create Window Title with date
$dt = Get-Date -Format D
$tit = "Message of the Day for " + $dt

## Display MessageBox
$Msg = [string]::join([environment]::newline, (Get-Content -LiteralPath $MsgFilePath))
$MsgB = $Msg.Replace("ARCHIVE - Delete this line if editing file", "")

$result = & $MsgBox $tit $MsgB "YesNo" "Information"

## Act on result
if ($result -eq "No") {
    Write-Host "User Pressed: $result"
}

if ($result -eq "Yes") {
    Write-Host "User Pressed: $result"
}
```

<strong>Message.txt</strong>

Message.txt is the file that is read by the MOTD.ps1 script and displayed to the user.  If you edit the file and see the archive message in the file, you must delete the archive message...

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-message-of-the-day/message.png" width="476" height="164" alt="message">

This will ensure the new message is archived by the script.  Currently the script assumes there will be only one message a day, so if a change is made to the script it will overwrite that day's archived message.  The message.txt is stored in the localcache folder on the server in the Message folder.  The script will create an Archive folder under the Message folder if none is present.

<strong>Script Logic Flow</strong>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-message-of-the-day/motd1.png" width="619" height="931" alt="motd1">

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*