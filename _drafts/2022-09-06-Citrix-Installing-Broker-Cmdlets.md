---
layout: post
title: "Installing Citrix Broker PowerShell Cmdlets"
date: 2022-09-06
tags: [Citrix,PowerShell]
readtime: true
cover-img: ["/assets/img/Citrix-Installing-Broker-Cmdlets/2022-09-06-Installing-Broker-Cmdlets.jpg" : "Pixabay"]
thumbnail-img: /assets/img/Citrix-Installing-Broker-Cmdlets/2022-09-06-Installing-Broker-Cmdlets.jpg
share-img: /assets/img/Citrix-Installing-Broker-Cmdlets/2022-09-06-Installing-Broker-Cmdlets.jpg
---

<!--more-->

# Contents

* TOC
{:toc}

# Scenario
Citrix has provided PowerShell management of its various products for years. In this post we will review the Citrix broker cmdlets and see how to install them.<br>
> Note: This post is written about the PowerShell cmdlets available with Citrix Virtual Apps and Desktops 1912

You can manage all aspects of a Citrix Site with the Citrix Broker SDK PowerShell cmdlets. This includes Machine Catalogs, Delivery Groups, Virtual Desktops, Applications, Database connections, user Sessions, and Tags.

# Prerequisites
* PowerShell 3.0
* The account running the cmdlets should be a [**Citrix Administrator**](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/secure/delegated-administration.html) (best practice is to not also have local admin rights to the server/system)
* To use Citrix PowerShell cmdlets in scripts, you will have to set your [**Execution Policy**](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy) to something other than Restricted

# Install Citrix PSSnapIns (Studio)
When you install Citrix Studio, all the PowerShell cmdlets for managing Virtual Apps and Desktops are installed as well. You can see the cmdlets in action within the Studio console. Studio generates the equivalent PowerShell commands for GUI actions in the PowerShell tab. You can review these by clicking on your Citrix Site.<br>
![Citrix Studio](/assets/img/Citrix-Installing-Broker-Cmdlets/brokercmdlets1.png "Citrix Studio")

Then click on the PowerShell tab to see the commands.
![PowerShell Tab](/assets/img/Citrix-Installing-Broker-Cmdlets/brokercmdlets2.png "PowerShell Tab")

You can click on the **Launch PowerShell** button the Citrix Studio to launch an elevated PowerShell Prompt with the Citrix cmdlets loaded.
![Studio Shell](/assets/img/Citrix-Installing-Broker-Cmdlets/brokercmdlets4.png "Studio Shell")

If you open a PowerShell prompt on a system where Citrix Studio is installed (this most often is the Delivery Controller) you will have to load the Citrix Cmdlets by typing this: `Add-PSSnapin Citrix.*`. This will install all the cmdlets for Citrix Virtual Apps and Desktops version 7.x and newer. Version 1 of the cmdlets are for XenDesktop 5.

# Install Citrix PSSnapIns (Manual)
If you wanted to install the Citrix PowerShell cmdlets on a system manually you can install them from the ISO.

The PowerShell Snapin MSI files are located in `Citrix Desktop Delivery Controller` folder in the `x64` directory (or `x86` directory - install the appropriate files for your OS). There are a number of MSI files in this folder, but you only need to install the MSI's that have `PowerShellSnapIn` in the filename.

```posh
PS E:\x64...\Citrix Desktop Delivery Controller> gci | where {$_.name -match 'PowerShellSnapin'} | select name


Name
----
ADIdentity_PowerShellSnapIn_x64.msi
Analytics_PowerShellSnapIn_x64.msi
AppLibrary_PowerShellSnapIn_x64.msi
Broker_PowerShellSnapIn_x64.msi
Configuration_PowerShellSnapIn_x64.msi
ConfigurationLogging_PowerShellSnapIn_x64.msi
DelegatedAdmin_PowerShellSnapIn_x64.msi
EnvTest_PowerShellSnapIn_x64.msi
Host_PowerShellSnapIn_x64.msi
MachineCreation_PowerShellSnapIn_x64.msi
Monitor_PowerShellSnapIn_x64.msi
Orchestration_PowerShellSnapIn_x64.msi
Storefront_PowerShellSnapIn_x64.msi
Trust_PowerShellSnapIn_x64.msi
UserProfileManager_PowerShellSnapIn_x64.msi
```

# Citrix Cmdlet Listing
A comprehensive list of Citrix cmdlets is available from [Citrix Developer Docs - 1912](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/1912/). Older and newer versions are available at the same site.

# Conculsion
If you have yet to dive into PowerShell cmdlets, I encourage you to do so sooner than later. There are numberous Citrix professionals sharing solutions using these cmdlets. You will not have to search long in order to find some ideas or even finished solutions available on Github.


# Learning More
* [Citrix Virtual Apps and Desktops 1912 SDK](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/1912/)
    * [Getting Started Guide](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/1912/getting-started/)
* [Citrix PowerShell SDK Documentation](https://citrix.github.io/delivery-controller-sdk/)
* [Set-ExecutionPolicy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy)


*Thanks for Reading,*
*Alain Assaf*
