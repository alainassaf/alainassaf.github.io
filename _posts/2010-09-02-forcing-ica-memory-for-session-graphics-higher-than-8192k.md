---
layout: post
title: Forcing ICA Memory for Session Graphics higher than 8192k
subtitle: Using PowerShell
date: 2010-09-02
tags: [Administration, Citrix XenApp, ICA, PowerShell, Remote Access, Res Wisdom, Scripting, Windows Registry, XenApp]
---
# Intro

In our XenApp environment, we support a desktop running dual 22” screens at a resolution of 1680x1050 (which is the standard desktop at work).  We can use a simple formula to calculate the amount of session graphics memory we need to support this screen setup.


<strong><span style="text-decoration:underline;">Session Graphics Memory Calculation Table</span></strong>
<table border="1" cellspacing="0" cellpadding="3" width="437">
<tbody>
<tr>
<td width="98" valign="top">X</td>
<td width="337" valign="top">Width of the ICA session window</td>
</tr>
<tr>
<td width="103" valign="top">Y</td>
<td width="333" valign="top">Height of the ICA session window</td>
</tr>
<tr>
<td width="107" valign="top">Z</td>
<td width="330" valign="top">Color Depth of the ICA session  window
(<strong>1</strong>-8 bit, <strong>2</strong>-16 bit,  <strong>3</strong>-24 bit)</td>
</tr>
<tr>
<td width="110" valign="top">M</td>
<td width="328" valign="top">Session Graphics Memory Required</td>
</tr>
<tr>
<td width="112" valign="top">Formula</td>
<td width="326" valign="top">M=X*Y*Z</td>
</tr>
</tbody>
</table>


<strong><span style="text-decoration:underline;">Default LCD Screen Resolutions:</span></strong>
<table border="1" cellspacing="0" cellpadding="3" width="283">
<tbody>
<tr>
<td width="136" valign="top"><strong>Standard </strong></td>
<td width="145" valign="top"><strong>Widescreen </strong></td>
</tr>
<tr>
<td width="136" valign="top">17" - 1024 x 768</td>
<td width="146" valign="top">19" - 1280 x 1024</td>
</tr>
<tr>
<td width="136" valign="top">19" - 1280 x 1024</td>
<td width="146" valign="top">22" - 1680 x 1050</td>
</tr>
<tr>
<td width="136" valign="top"></td>
<td width="146" valign="top">24" - 1920 x 1200</td>
</tr>
</tbody>
</table>

Calculating Dual 22” screens at 1680x1050 = M = (2 * 1680) x 1050 x 3 =  10584000 or 10,584k

The exceeds the standard size for XenApp Session graphics which is 8,192k.  Citrix outlines how to change this setting in a server’s registry in <a title="How to Allow More Memory for Session Graphics on Windows Server 2003" href="http://support.citrix.com/article/CTX114497" target="_blank">CTX114497</a>.  We found, however, that after we set this value, XenApp would set it back to the standard size.

<strong>PowerShell to the rescue</strong>
It turns out that the permissions were not set correctly on the registry key.  You have to explicitly deny SYSTEM the ability to SETVALUE on the key, otherwise the system changes the ICA Session Graphics back to the default farm settings.  The current method (Res Wisdom) we used to set the value did not have enough granularity to properly set the registry key permissions.

A quick Google search struck pay dirt in an on-line eBook by no less a PowerShell luminary than <a href="https://mvp.support.microsoft.com/profile/Weltner" target="_blank">Dr. Tobias Weltner</a>.  Using this as a reference: <a href="http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-16-the-registry.aspx#permissions-in-the-registry">http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-16-the-registry.aspx#permissions-in-the-registry</a>, I wrote 2 PowerShell routines to manipulate the permissions on this key.

The first removes DENY permissions for the SYSTEM account, which in our case allows Res Wisdom to change the setting if needed (Wisdom uses SYSTEM to change/query registry settings).

```posh
# =========================================================================================================================
# AUTHOR: Alain Assaf
# NAME:   unset-permissions-icawd.ps1
# DATE:   7/26/2010
#
# SOURCE: http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-16-the-registry.aspx#permissions-in-the-registry
# COMMENT: Resets HKLM key below to allow for permission inheritance so value can be set by SYSTEM
# =========================================================================================================================

# Create $acl and point to registry key
$acl = Get-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16"
$acl.SetAccessRuleProtection($false, $true)
Set-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16" $acl

# Remove Deny on SetValue for SYSTEM:
$person = [System.Security.Principal.NTAccount]"SYSTEM"
$access = [System.Security.AccessControl.RegistryRights]"SetValue"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Deny"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.RemoveAccessRule($rule)
Set-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16" $acl

# Remove Deny on FullControl for SYSTEM:
$person = [System.Security.Principal.NTAccount]"SYSTEM"
$access = [System.Security.AccessControl.RegistryRights]"FullControl"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Deny"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.RemoveAccessRule($rule)
Set-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16" $acl
```

The second runs after the registry settings are set and re-establishes the proper permissions to let SYSTEM read the key, but not change it.

```posh
# =========================================================================================================================
# AUTHOR: Alain Assaf
# NAME:   set-permissions-icawd.ps1
# DATE:   7/26/2010
#
# SOURCE: http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-16-the-registry.aspx#permissions-in-the-registry
# COMMENT: Sets proper permissions on hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16
#          to allow for dual 22" screens for XenApp users.
# =========================================================================================================================

# Create $acl
$acl = Get-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16"

# Grant Administrators get Full Control:
$person = [System.Security.Principal.NTAccount]"Administrators"
$access = [System.Security.AccessControl.RegistryRights]"FullControl"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Allow"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.ResetAccessRule($rule)

# Grant Users Read Access:
$person = [System.Security.Principal.NTAccount]"Users"
$access = [System.Security.AccessControl.RegistryRights]"ReadKey"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Allow"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.AddAccessRule($rule)

# Grant SYSTEM gets Full Control...
$person = [System.Security.Principal.NTAccount]"SYSTEM"
$access = [System.Security.AccessControl.RegistryRights]"FullControl"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Allow"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.AddAccessRule($rule)

# ...except for SETVALUE:
$person = [System.Security.Principal.NTAccount]"SYSTEM"
$access = [System.Security.AccessControl.RegistryRights]"SetValue"
$inheritance = [System.Security.AccessControl.InheritanceFlags]"None"
$propagation = [System.Security.AccessControl.PropagationFlags]"None"
$type = [System.Security.AccessControl.AccessControlType]"Deny"
$rule = New-Object System.Security.AccessControl.RegistryAccessRule($person,$access,$inheritance,$propagation,$type)
$acl.AddAccessRule($rule)
$acl.SetAccessRuleProtection($true, $false)

# Apply new ACL on registry key
Set-Acl "hklm:\system\currentcontrolset\control\terminal server\wds\icawd\thin16" $acl
```

Now XenApp cannot change this setting and it sticks upon subsequent reboots or changes to the XenApp farm settings.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*