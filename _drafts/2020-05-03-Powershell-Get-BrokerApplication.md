---
layout: post
title: "Citrix: Get-BrokerApplication"
date: 2020-05-03
tags: [Citrix,PowerShell,Report]
bigimg:
    - "/img/2020-05-03-Get-BrokerApplication.jpg" : "Pixabay"
---

Citrix has provided PowerShell management of its various products for years. In this post we will look at Get-BrokerApplication, which retrieve the applications published on this site.

<!--more-->

# Index

* TOC
{:toc}

# Scenario
I have many, many published applications to manage. I have to make changes monthly (sometimes weekly) to existing applications, create new applications, and provide testing applications for various application and business groups. I want to use Get-BrokerApplication to generate a quick report to show me the 'types' of applications I give me an overview of application changes from month-to-month.

# Install Citrix PSSnapIns (Studio)
When you install Citrix Studio, all the PowerShell cmdlets for managing Virtual Apps and Desktops are installed as well. You can see the cmdlets in action within the Studio console. Studio generates the equivalent PowerShell commands for GUI actions in the PowerShell tab. You can review these by clicking on your Citrix Site. 
![Citrix Studio](/img/gbm1.png "Citrix Studio")

Then click on the PowerShell tab to see the commands.
![PowerShell Tab](/img/gbm2.png "PowerShell Tab")

When you open a PowerShell prompt on a system where Citrix Studio is installed (this most often is the Delivery Controller) you can load the Citrix Cmdlets by typing this: `Add-PSSnapin Citrix.*.Admin.V*`    

# Install Citrix PSSnapIns (Manual)
If you wanted to install the Citrix PowerShell cmdlets on a system manually you can install them from the ISO.
The **Get-BrokerApplication** cmdlet is part of the *Citrix.Broker.Admin.V2* PSSnapIn.

# Get-BrokerApplication

# The code
```posh
$max = [int]::MaxValue

$apps = Get-BrokerApplication -AdminAddress $DeliveryController -MaxRecordCount $max
Write-host "Total apps:" $apps.Count
Write-host "Enabled apps:" ($apps | where {$_.Enabled -eq $True}).count
Write-host "Disabled apps:" ($apps | where {$_.Enabled -eq $False}).count
Write-host "Patch Testing (All):" ($apps | where {$_.applicationname -match 'PT-'}).count
Write-host "Patch Testing (Folder):" ($apps | where {$_.Adminfoldername -eq 'Patch Testing\'}).count
Write-host "Edge Patch Testing apps:" ($apps | where {$_.Adminfoldername -eq 'Patch Testing-Edge\'}).count
write-host "'Test' apps:" ($apps | where {$_.applicationname -match 'test'}).count
write-host "'Prod' apps:" ($apps | where {$_.applicationname -match 'prod'}).count
write-host "'Staging' apps:" ($apps | where {$_.applicationname -match 'staging'}).count
write-host "'QA' apps:" ($apps | where {$_.applicationname -match 'qa'}).count
write-host "'IE' apps:" ($apps | where {$_.CommandLineExecutable -match 'iexplore'}).count
write-host "'Chrome' apps:" ($apps | where {$_.CommandLineExecutable -match 'chrome'}).count
write-host "'Edge' apps:" ($apps | where {$_.CommandLineExecutable -match 'msedge'}).count
write-host "'Office' apps:" ($apps | where {$_.CommandLineExecutable -match 'office'}).count
```

# Conculsion


# Learning More
* [Get-BrokerApplication](https://citrix.github.io/delivery-controller-sdk/Broker/Get-BrokerApplication/)


*Thanks for Reading,*

*Alain Assaf*
