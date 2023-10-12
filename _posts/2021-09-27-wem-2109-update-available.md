---
layout: post
title: WEM 2109 UPDATE AVAILABLE
date: 2021-09-27
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2109-update-available/2kj615.jpg
share-img: /assets/img/wem-2109-update-available/2kj615.jpg
---
![provemewrong](/assets/img/wem-2109-update-available/2kj615.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fall is here and so is a new pumpkin-spice flavor of WEM. Citrix has released version 2109 of WEM. You can download the new version <a rel="noreferrer noopener" href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2109.html" target="_blank">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2109">What’s new in 2109<a href="void(0)"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2109 includes the following new features. For information about bug fixes, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">Fixed issues</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="support-for-running-workspace-environment-management-in-fips-mode">Support for running Workspace Environment Management in FIPS mode</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can now run Workspace Environment Management (WEM) in a FIPS environment. See&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/reference/fips-mode.html">FIPS support</a>&nbsp;for information on FIPS-related configurations in WEM and upgrade and agent considerations.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Fixed Issues</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2109 contains the following fixed issues compared to Workspace Environment Management 2106:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>If you assign a printer to a user based on a filter and the assignment satisfies the filter criteria, the WEM agent assigns the printer to the user. However, the agent still assigns the printer to the user the next time the user logs on even when the assignment does not satisfy the filter criteria. [WEM-11680]</li><li>With the Windows PowerShell script execution policy set to&nbsp;<strong>Allow only signed scripts</strong>&nbsp;on the agent host machine, WEM fails to perform Profile Management health checks. With the policy set to&nbsp;<strong>Allow local scripts and remote signed scripts</strong>&nbsp;or&nbsp;<strong>Allow all scripts</strong>, WEM can perform Profile Management health checks but writes error information to the Windows Event Log. [WEM-11917]</li><li>On a non-English version of the Microsoft Windows operating system, the WEM agent during logon writes errors to the Windows Event Log even if users experience no issues with their environment. [WEM-12603]</li><li>When you assign an action to a user or user group through an action group, the action still takes effect even if it is set to&nbsp;<strong>Disabled</strong>&nbsp;in the administration console. [WEM-12757]</li><li>The WEM agent installs VUEMRSAV.exe (<strong>Workspace Environment Management Resultant Actions Viewer</strong>), a utility that lets users view the WEM configuration defined for them by administrators. However, on the&nbsp;<strong>Agent Settings</strong>&nbsp;tab of the utility, users cannot see the setting that is associated with the&nbsp;<strong>Use Cache to Accelerate Actions Processing</strong>&nbsp;option configured in the administration console. [WEM-12847]</li><li>Attempts to upgrade the WEM database from version 2003 or earlier to version 2009 or later might fail if you specify a different schema (not&nbsp;<strong>dbo</strong>) as the default schema. [WEM-13319]</li><li>If you use the [ADAttribute:objectSid] dynamic token to extract the objectsid attribute, the WEM agent fails to extract the attribute of the corresponding AD object. [WEM-13746]</li><li>When you attempt to configure Windows Server 2022 as the matching result of a filter condition (with&nbsp;<strong>Client OS</strong>&nbsp;as the filter condition type), you find that Windows Server 2022 is missing from the&nbsp;<strong>Matching Result</strong>&nbsp;menu. [WEM-14036]</li><li>When the WEM agent belongs to an OU or a group whose name contains certain characters (for example, forward slash, plus sign, and equal sign), the agent might fail to register with a configuration set. As a result, the agent fails to appear on the&nbsp;<strong>Administration console &gt; Administration &gt; Agents &gt; Registrations</strong>&nbsp;tab. [WEM-14316]</li><li>If you use the administration console to set desktop wallpaper, the WEM agent fails to fill, fit, or tile the wallpaper. [WEM-14408]</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2>Deprecation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"normal"} -->
<p class="has-normal-font-size">Click here to see <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/deprecation.html">deprecated features</a>.</p>
<!-- /wp:paragraph -->

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

<!-- wp:paragraph -->
<p><em>Thanks for reading,<br />
Alain</em></p>
<!-- /wp:paragraph -->
