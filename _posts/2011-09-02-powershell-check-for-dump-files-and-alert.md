---
layout: post
title: Check for Dump Files and Alert
subtitle: PowerShell
date: 2011-09-02
tags: [Administration, memory dump, PowerShell, Provisioning Services, Reporting, Monitoring]
cover-img: ["/assets/img/powershell-check-for-dump-files-and-alert/bruce-mars-FWVMhUa_wbY-unsplash.jpg" : "Photo by Bruce Mars on Unsplash"]
thumbnail-img: /assets/img/powershell-check-for-dump-files-and-alert/bruce-mars-FWVMhUa_wbY-unsplash.jpg
share-img: /assets/img/powershell-check-for-dump-files-and-alert/bruce-mars-FWVMhUa_wbY-unsplash.jpg
---
<strong>Intro</strong>

<img style="display:inline;float:left;" src="/assets/img/icons/powerShellIcon.jpg" alt="" align="left" />Citrix Provisioning Services has become a cornerstone technology in many XenApp and XenDesktop deployments.  Provisioned devices rely on a <em><a href="https://support.citrix.com/article/CTX119469/understanding-write-cache-in-provisioning-services-server" target="_blank"><b>write-cache</b></a></em> to store new data while they are running and Citrix best practice recommends pointing pagefiles, event logs, and other write-intensive files to this write-cache.  This also includes EdgeSight data.  The one draw back is that if the write-cache fills up, the provisioned device stops working.  I’ve written a simple PowerShell script that will scan the write-cache for DMP files and the dedicateddumpfile.sys and send an e-mail alert if something is found.

<strong>The script</strong>

```posh
###############################################################################
## Title       : check-dmpfiles.ps1
## Description : Checks writecache drive for dmp files and e-mails if found
## Author      : Alain Assaf
## Date        : 08/30/2011
## Notes       :
#### Changelog ################################################################
# -When - What - Who
# -08/30 -Initial script - Alain Assaf
###############################################################################

$emailBody=$null
$dmplocation=$null
$dmplocation="d:\"
$searchfor = @("*.dmp","dedicateddumpfile.sys")

### Script Body ###############################################################
$results = Get-ChildItem $dmplocation -Include $searchfor -Recurse -Force -ErrorAction SilentlyContinue
if (!$results) {
    exit
} else {
    foreach ($file in $results) {
        $dmpfile = (Get-ChildItem $file -force).Name
        $dmpfilesize = '{0:N2}' -f (((Get-Item $file -force).length) / 1048576.0)
        $dmpfiledir = (Get-ChildItem $file -force).DirectoryName
        $emailBody = $emailBody + "`nFound: $dmpfile"
        $emailBody = $emailBody + "`nSize: $dmpfilesize MB"
        $emailBody = $emailBody + "`nIn directory: $dmpfiledir"
        $emailBody = $emailBody + "`n------------------------------------------------------------------------------"
    }
    $drvroot = (Get-Item $file -force).psdrive.Root
    $drvfreespace = '{0:N2}' -f ((Get-Item $file -force).psdrive.Free / 1073741824.0)
    $emailBody = $emailBody + "`n$drvroot Drive free space: $drvfreespace GB"
}

### E-mail Creation: Variables, Sending #######################################
#$emailFrom = "dmpfilecheck@company.com"
$emailTo ="sbcteam@company.com"
$subject = "ALERT: Dump file found on $hostname"
$smtpServer = "192.168.1.1"
$smtp = new-object Net.Mail.SmtpClient($smtpServer)
$msg = new-object Net.Mail.MailMessage($emailFrom, $emailTo, $subject, $emailBody)
if ($emailBody) {$smtp.send($msg)}
```

<strong>Script Logic Flow</strong>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-check-for-dump-files-and-alert/check-dmpfiles-script-logic-flow.png" alt="image">

Here’s an example e-mail:

```
Assaf, Alain
From: dmpfilecheck
Sent: Thursday, September 01, 2011 9:19 PM
To: Assaf, Alain
Subject: ALERT: Dump file found on XENAPPPVS

Found: rscorsvc.exe.dmp
Size: 86.65 MB
In directory: D:\Citrix\System Monitoring\Data
------------------------------------------------------------------------------
Found: rscorsvc.exe.hang.dmp
Size: 95.80 MB
In directory: D:\Citrix\System Monitoring\Data
------------------------------------------------------------------------------
Found: rscorsvc.exe.shutdown.dmp
Size: 66.71 MB
In directory: D:\Citrix\System Monitoring\Data
------------------------------------------------------------------------------
D:\ Drive free space: 9.24 GB
```

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*