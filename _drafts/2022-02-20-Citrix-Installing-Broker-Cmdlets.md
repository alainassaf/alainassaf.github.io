---
layout: post
title: "Installing Citrix Broker PowerShell Cmdlets"
date: 2022-02-20
tags: [Citrix,PowerShell]
bigimg:
    - "/img/2022-02-20-Installing-Broker-Cmdlets.jpg" : "Pixabay"
---

Citrix has provided PowerShell management of its various products for years. In this post we will review the cmdlets and see how to install them.<br>
> Note: This post is written about the PowerShell cmdlets available with Citrix Virtual Apps and Desktops 1912

<!--more-->

# Index

* TOC
{:toc}

# Intro
You can manage all aspects of a Citrix Site with the Citrix Broker SDK PowerShell cmdlets. This includes Machine Catalogs, Delivery Groups, Virtual Desktops, Applications, Database connections, user Sessions, and Tags.

# Prerequisites
* PowerShell 3.0
* The account running the cmdlets should be a [**Citrix Administrator**](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/secure/delegated-administration.html) (best practice is to not also have local admin rights to the server/system)
* To use Citrix PowerShell cmdlets in scripts, you will have to set your [**Execution Policy**](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy) to something other than Restricted

# Install Citrix PSSnapIns (Studio)
When you install Citrix Studio, all the PowerShell cmdlets for managing Virtual Apps and Desktops are installed as well. You can see the cmdlets in action within the Studio console. Studio generates the equivalent PowerShell commands for GUI actions in the PowerShell tab. You can review these by clicking on your Citrix Site.<br>
![Citrix Studio](/img/brokercmdlets1.png "Citrix Studio")

Then click on the PowerShell tab to see the commands.
![PowerShell Tab](/img/brokercmdlets2.png "PowerShell Tab")

You can click on the **Launch PowerShell** button the Citrix Studio to launch an elevated PowerShell Prompt with the Citrix cmdlets loaded.
![Studio Shell](/img/brokercmdlets4.png "Studio Shell")

If you open a PowerShell prompt on a system where Citrix Studio is installed (this most often is the Delivery Controller) you will have to load the Citrix Cmdlets by typing this: `Add-PSSnapin Citrix.ADIdentity.Admin.V2`. This will install the cmdlets for Citrix Virtual Apps and Desktops version 7.x and newer. Version 1 of the cmdlets are for XenDesktop 5.

# Install Citrix PSSnapIns (Manual)
If you wanted to install the Citrix PowerShell cmdlets on a system manually you can install them from the ISO.


# Broker Cmdlet Listing

# The code
```posh

```

# Conculsion


# Learning More
* [SDKs and APIs](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/1912/sdk-api.html)
* [Citrix PowerShell SDK Documentation](https://citrix.github.io/delivery-controller-sdk/)
* [Set-ExecutionPolicy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy)


*Thanks for Reading,*

*Alain Assaf*
