---
layout: post
title: Enabling or Disabling HDX Plug-n-Play for USB Storage Devices
date: 2011-12-13
tags: [Administration, Citrix, Documentation, Remote Access, XenApp]
---
<strong>Intro</strong>

Sometimes your security team will be your best friend, and other times they will be the "bad guy" in your IT organization.  They will prevent certain applications from being installed or they will require that remote users have reduced access. Recently, I was approached by my security team to prevent access to the clipboard, local printers, and local drives for a certain group of users.  This better secures our environment and  reduces ICA bandwidth and speeds up login times. When I created a new Citrix policy to put these restrictions in place, I found that USB hard-drives were still being mapped.

## Solution 
Searching the Citrix eDocs site, I came across the following detail at the bottom of the Drives Folder section in the [**Policy Rules Reference**](http://support.citrix.com/proddocs/topic/xenapp5fp2-w2k3/ps-console-policies-rules-client-drives-v3.html){:target="_blank"}:

> **Enabling or Disabling HDX Plug-n-Play for USB Storage Devices**
> 
> HDX Plug-n-Play for USB storage devices is enabled by default. To change the settings for HDX Plug-n-Play for USB storage devices, manually change the key specified below on the XenApp server. Changes apply to all users.
> 
> **Caution**: Using Registry Editor incorrectly can cause serious problems that can require you to reinstall the operating system. Citrix cannot guarantee that problems resulting from incorrect use of Registry Editor can be solved. Use Registry Editor at your own risk. Make sure you back up the registry before you edit it.
> 
> Toggle USB drive redirection on and off using the following registry key on the server:
> 
> **On XenApp 32-bit edition**  
> `HKEY_LOCAL_MACHINE\Software\Citrix\Policies\DisableUSBDriveRedirection`
> 
> **On XenApp 64-bit edition**  
> `HKEY_LOCAL_MACHINE\Software\Wow6432Node\Citrix\Policies\DisableUSBDriveRedirection`
> 
> | Type | DWORD |
> | Values | 1 = redirection disabled |
> | | 0 = redirection enabled |
> 
> ***Note***: HDX Plug-n-Play for USB storage devices is enabled when the registry key is not present.

## Result 
Once I added the registry setting (and logged back in), I was no longer able to see any mapped USB hard drives. This is also referenced in the following CTX articles: 
+ [**How to Prevent Manual Mapping of Client Connected USB Drives**](http://support.citrix.com/article/CTX130472){:target="_blank"}
+ [**How to Disable USB Drive Redirection**](http://support.citrix.com/article/CTX123700){:target="_blank"}


### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*