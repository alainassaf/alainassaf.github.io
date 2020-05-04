---
layout: post
title: "Citrix: Installing Broker Cmdlets"
date: 2020-05-03
tags: [Citrix,PowerShell]
bigimg:
    - "/img/2020-05-03-Installing-Broker-Cmdlets.jpg" : "Pixabay"
---

Citrix has provided PowerShell management of its various products for years. In this post we will look at Get-BrokerApplication, which retrieve the applications published on this site.

<!--more-->

# Index

* TOC
{:toc}

# Intro


# Install Citrix PSSnapIns (Studio)
When you install Citrix Studio, all the PowerShell cmdlets for managing Virtual Apps and Desktops are installed as well. You can see the cmdlets in action within the Studio console. Studio generates the equivalent PowerShell commands for GUI actions in the PowerShell tab. You can review these by clicking on your Citrix Site. 
![Citrix Studio](/img/brokercmdlets1.png "Citrix Studio")

Then click on the PowerShell tab to see the commands.
![PowerShell Tab](/img/brokercmdlets2.png "PowerShell Tab")

When you open a PowerShell prompt on a system where Citrix Studio is installed (this most often is the Delivery Controller) you can load the Citrix Cmdlets by typing this: `Add-PSSnapin Citrix.*.Admin.V*`    

# Install Citrix PSSnapIns (Manual)
If you wanted to install the Citrix PowerShell cmdlets on a system manually you can install them from the ISO.
The **Get-BrokerApplication** cmdlet is part of the *Citrix.Broker.Admin.V2* PSSnapIn.

# The code
```posh

```

# Conculsion


# Learning More
* [SDKs and APIs](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/sdk-api.html)


*Thanks for Reading,*

*Alain Assaf*
