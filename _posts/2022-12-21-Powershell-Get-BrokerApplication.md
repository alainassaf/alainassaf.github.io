---
layout: post
title: "Citrix: Get-BrokerApplication"
date: 2022-12-21
readtime: true
tags: [Citrix,PowerShell,Report]
cover-img: ["/assets/img/Powershell-Get-BrokerApplication/powershell-get-brokerapplication.jpg" : "Pixabay"]
thumbnail-img: /assets/img/Powershell-Get-BrokerApplication/powershell-get-brokerapplication.jpg
share-img: /assets/img/Powershell-Get-BrokerApplication/powershell-get-brokerapplication.jpg
---

Citrix has provided PowerShell management of its various products for years. In this post, we will look at Get-BrokerApplication, which retrieves the applications published in a Citrix farm.

<!--more-->

# Index

* TOC
{:toc}

# Scenario
I have many, many published applications to manage. In a given week, I may have to make changes to existing applications, create new applications, and provide testing applications for various applications and business groups. I want to use Get-BrokerApplication to generate a quick report to show me the 'types' of applications I have. Using this info, I can track application changes from month-to-month.

# Install Citrix PSSnapIns (Studio)
When you install Citrix Studio, all the PowerShell cmdlets for managing Virtual Apps and Desktops are installed as well. You can see the cmdlets in action within the Studio console. Studio generates the equivalent PowerShell commands for GUI actions in the PowerShell tab. You can review these by clicking on your Citrix Site. 
![Citrix Studio](/assets/img/Powershell-Get-BrokerApplication/gbm1.png "Citrix Studio")

Then click on the PowerShell tab to see the commands.
![PowerShell Tab](/assets/img/Powershell-Get-BrokerApplication/gbm2.png "PowerShell Tab")

When you open a PowerShell prompt on a system where Citrix Studio is installed (this most often is the Delivery Controller) you can load the Citrix Cmdlets by typing this: `Add-PSSnapin Citrix.*.Admin.V*`    

# Install Citrix PSSnapIns (Manual)
If you wanted to install the Citrix PowerShell cmdlets on a system manually, you can install them from the ISO.
Open the Citrix Virtual Apps and Desktops ISO image and go to the `.\x64\Citrix Desktop Delivery Controller` directory. There will be numerous  MSI files. 
![MSI Files](/assets/img/Powershell-Get-BrokerApplication/gbm3.png) "MSI Files")

You will have to install all the MSI that are named `*_PowerShellSnapIn_x64.msi*` and include `XDPoshSnap_x64.msi`. Here's a simple script to install these MSI's

```posh
#Assuming the ISO is mounted under the drive letter E
Set-Location "e:\x64\Citrix Desktop Delivery Controller"
Get-ChildItem * | where {$_.name -match "SnapIn"} | ForEach-Object {start-process "$_" -Wait}
```

# Making a simple application report
The **Get-BrokerApplication** cmdlet is part of the *Citrix.Broker.Admin.V2* PSSnapIn. You can grab all your applications in your farm with the following commands:
```posh
$max = [int]::MaxValue
$DeliveryController = "MYCITRIXDDC"
$apps = Get-BrokerApplication -AdminAddress $DeliveryController -MaxRecordCount $max
```

`$max = [int]::MaxValue` sets the `$max` variable to the largest integer number which, in this case is 2147483647. This my perfered way of getting around the default value of 250 for this parameter, of course this only matters if you have over 250 applications in your Citirx farm.

After running this command, the `$apps` variable will have all of your applications. You can then filter them based on whether they are enabled or disabled, if they are URLs for Edge or Internet Explorer, or if they use one of the office applications.

# The code
```posh
$max = [int]::MaxValue
$DeliveryController = "MYCITRIXDDC"
$apps = Get-BrokerApplication -AdminAddress $DeliveryController -MaxRecordCount $max
Write-host "Total apps:" $apps.Count
Write-host "Enabled apps:" ($apps | where {$_.Enabled -eq $True}).count
Write-host "Disabled apps:" ($apps | where {$_.Enabled -eq $False}).count
Write-host "Patch Testing (Folder):" ($apps | where {$_.Adminfoldername -eq 'Patch Testing\'}).count
write-host "Test apps:" ($apps | where {$_.applicationname -match 'test'}).count
write-host "Prod apps:" ($apps | where {$_.applicationname -match 'prod'}).count
write-host "QA apps:" ($apps | where {$_.applicationname -match 'qa'}).count
write-host "IE apps:" ($apps | where {$_.CommandLineExecutable -match 'iexplore'}).count
write-host "Chrome apps:" ($apps | where {$_.CommandLineExecutable -match 'chrome'}).count
write-host "Edge apps:" ($apps | where {$_.CommandLineExecutable -match 'msedge'}).count
write-host "Office apps:" ($apps | where {$_.CommandLineExecutable -match 'office'}).count
```

# Report
Putting all these queries together can in a script can give you simple summary report of the applications in your Citrix farm.

![Application Report](/assets/img/Powershell-Get-BrokerApplication/gbm4.png "Application Report")

# Conclusion
In this post, we showed how to install the Citrix PowerShell cmdlets and use `Get-BrokerApplication` to create a simple report of your Citrix applications. Running this script every month allows us to track changes over time. 

![Report Table](/assets/img/Powershell-Get-BrokerApplication/gbm5.png "Report Table")


# Learning More
* [Get-BrokerApplication](https://citrix.github.io/delivery-controller-sdk/Broker/Get-BrokerApplication/)


*Thanks for Reading,*
*Alain Assaf*
