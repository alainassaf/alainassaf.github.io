---
layout: post
title: Published Application Consistency
date: 2009-09-19
tags: [Administration, Citrix, PowerShell, Scripting]
---
Currently, in my XenApp environment, we maintain nearly 300 published applications and desktops.  We also are adding 2 or 3 new applications a month.  Managing this many resources  is cumbersome especially when you rely on Session Sharing.  Session Sharing is described in the Citrix knowledge base article [**Troubleshooting and Explaining Session Sharing**](http://support.citrix.com/article/CTX159159){:target="_blank"} and basically causes all application launches to occur on the same XenApp server or within the same user session.  This provides  a better experience for the user (since he does not have to wait for a new login to complete for every new application launched), in user management (since a user's session is only on one XenApp server) and in user profile stability (since the user isn't logged into multiple servers).

The main requirement for Session Sharing is to have the same application installed on all your servers.  The secondary requirements are less obvious, but listed in CTX159159.

They are:
+ Color depth
+ Screen Size
+ Access Control Filters (for SmartAccess)
+ Sound
+ Drive Mapping
+ Printer Mapping

The issue then becomes how does one review all their applications to determine that the above settings are consistent.  Pouring over the application properties in the AMC is a recipe for insanity (in my opinion).  In this post, will explore a couple of ways to ensure your applications settings are consistent.

**Brute Force**

The site [**Citrixtools.net**](http://web.archive.org/web/20090915231029/http://www.citrixtools.net/en/Home.aspx){:target="_blank"} provides a terrific resource to the French and international  Citrix community.  They have provided several terrific tools one of which is called [**XenApp App Manager**](http://web.archive.org/web/20111208065018/http://www.citrixtools.net/Downloads.aspx){:target="_blank"}

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 30%;"
    src="/assets/img/published-application-consistency/appmanager.jpg" 
    alt="appmanager">

With this tool you can apply a variety of application properties to as many or as few applications as you want, but this tool will not report on the current settings of your applications.  Also, this tool does not address Access Control Filters, Drive Mapping, or Printer Mapping.  So how do you gather this information?

<strong>PowerShell to the rescue</strong>

Citrix has fully embraced PowerShell (thankfully) with the development of [**Workflow Studio**](https://www.citrix.com/downloads/xenapp/components/workflow-studio.html#:~:text=Citrix%20Workflow%20Studio%E2%84%A2%20is%20an%20infrastructure%20process%20automation,you%20in%20installing%20the%20product%20and%20activity%20libraries.) 

Version 2.0  was released this week.  At Synergy 2009, they also released a tech preview for a variety of XenApp PowerShell cmdlets (download the preview from this `http://www.citrix.com/English/ps2/products/feature.asp?contentID=1854441`.  These commands, while simple are very powerful and we can utilize one of them to output all the important settings necessary for consistent Session Sharing (see Brandon Shell's excellent site about [**PowerShell**](https://web.archive.org/web/20090226025744/http://bsonposh.com/){:target="_blank"}.

The following cmdlet will display all the application properties we need to check.

```posh
PS PoSh:> get-help Get-XAApplicationReport

NAME
Get-XAApplicationReport

SYNOPSIS
Gets detailed information for published applications.

SYNTAX
Get-XAApplicationReport [-BrowserName] [<String>] [-WhatIf] [-Confirm] [<CommonParameters>]

Get-XAApplicationReport -InputObject -XAApplication [-WhatIf] [-Confirm] [<CommonParameters>]

DETAILED DESCRIPTION
This cmdlet gets detailed information for published applications, including associations, such as servers, accounts, file types and icons.

RELATED LINKS
Get-XAApplication

REMARKS
For more information, type: "get-help Get-XAApplicationReport -detailed".
For technical information, type: "get-help Get-XAApplicationReport -full".
```

This command displays many properties, but to display the ones we are interested in for session sharing we'll use the following command:

```posh
Get-XAApplicationReport * | select DisplayName, WindowType, ColorDepth, ConnectionsThroughAccessGatewayAllowed, OtherConnectionsAllowed, AccessSessionConditionsEnabled, AudioType, AudioRequired, SslConnectionEnabled, EncryptionLevel, EncryptionRequired, WaitOnPrinterCreation
```

Piping this command to format-table -wrap will ensure that all the data will display.  After this, you can output to a text or spreadsheet file and compare all the properties to make sure they are consistent.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*