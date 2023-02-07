---
layout: post
title: User Personalization, Microsoft Outlook
subtitle: AppSense
date: 2010-06-18
tags: [AppSense, Environment Manager, User, Virtualization]
---
<strong>AppSense</strong> allows a user’s application customizations to move to any managed device.  The benefit of this for the user is obvious as it creates a seamless experience, reduces user frustration, and reduces service desk calls.  In this post, I’ll cover what I’m capturing to manage MS Outlook in AppSense.

### Application Tab

No surprises here, we’re going to capture the settings of outlook.exe.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-user-personalization-microsoft-outlook/image.png" width="630" height="484" alt="image">

### Registry Tab

This exclusion is from AppSense best practice.  The must include this registry key in the Desktop Settings.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-user-personalization-microsoft-outlook/image1.png" width="644" height="280" alt="image1">

### Folders Tab

<table border="0" cellspacing="0" cellpadding="2" width="577">
<tbody>
<tr>
<td width="200" valign="top">\Microsoft\Proof</td>
<td width="375" valign="top">Custom Dictionary</td>
</tr>
<tr>
<td width="200" valign="top">\Microsoft\Signatures</td>
<td width="375" valign="top">Outlook Signatures</td>
</tr>
<tr>
<td width="200" valign="top">\Microsoft\Office</td>
<td width="375" valign="top">AutoCorrect Entries</td>
</tr>
<tr>
<td width="200" valign="top">\Microsoft\Outlook</td>
<td width="375" valign="top">Name cache, Address Books, Menu customizations, etc.</td>
</tr>
</tbody>
</table>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-user-personalization-microsoft-outlook/image2.png" width="644" height="364" alt="image2">

For a further explanation of where Outlook stores user information, check out this article from [**OutlookPower magazine**](http://outlookpower.com/article/where-outlook-hides-its-secret-stuff/){:target="_blank"}.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*