---
layout: post
title: WEM 2103 UPDATE AVAILABLE
date: 2021-03-18
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2103-update-available/5277p3.jpg
share-img: /assets/img/wem-2103-update-available/5277p3.jpg
---
![Gamestop](/assets/img/wem-2103-update-available/5277p3.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>New Year, new WEM release. Citrix has released the first version of WEM for 2021, version 2103. You can download the new version <a rel="noreferrer noopener" href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2103.html" target="_blank">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2103">What’s new in 2103<a href="void(0)"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2103 includes the following new features. For information about bug fixes, see below.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="profile-management">Profile Management</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management now supports all versions of Profile Management through 2103. Also, the following new options are now available in the&nbsp;<strong>Administration Console &gt; Policies and Profiles &gt; Citrix Profile Management Settings</strong>&nbsp;interface:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Enable Local Cache for Profile Container</strong><ul><li>Available on the&nbsp;<strong>Profile Container Settings</strong>&nbsp;tab.</li><li>If enabled, each local profile serves as a local cache of its profile container.</li></ul></li><li><strong>Enable multi-session write-back for profile containers</strong><ul><li>Available on the&nbsp;<strong>Advanced Settings</strong>&nbsp;tab.</li><li>Replaces&nbsp;<strong>Enable multi-session write-back for FSLogix Profile Container</strong>&nbsp;of previous releases to accommodate multi-session write-back support for Citrix Profile Management profile containers.</li></ul></li><li><strong>Enable Profile Streaming for Folders</strong><ul><li>Available on the&nbsp;<strong>Streamed User Profiles</strong>&nbsp;tab.</li><li>If enabled, folders are fetched only when they are being accessed.</li></ul></li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/policies-and-profiles/citrix-upm-settings.html">Citrix Profile Management Settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="sdk-documentation">SDK documentation</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release updates PowerShell modules in the Workspace Environment Management SDK. The following cmdlets are no longer usable:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><code>Property SDKInfrastructureServiceConfiguration.AgentSyncPort</code></li><li><code>Property Commandlets.SetWemInfrastructureServiceConfiguration.AgentSyncPort</code></li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Version 2103 of the&nbsp;<a href="https://developer-docs.citrix.com/projects/workspace-environment-management-sdk/en/latest/" rel="noreferrer noopener" target="_blank">Workspace Environment Management SDK documentation</a>&nbsp;reflects the update.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Fixed Issues</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2103 contains the following fixed issues compared to Workspace Environment Management 2012:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>After you upgrade the WEM agent to version 1912, the memory consumption of&nbsp;<strong>Citrix WEM Agent Host Service</strong>&nbsp;might exceed 2G. If debug mode is enabled, you can see that the following messages appear many times in the&nbsp;<strong>Citrix WEM Agent Host Service Debug.log</strong>&nbsp;file:<ul><li><strong>Adding history entry to the DB writer queue</strong></li><li><strong>Initializing process limitation thread for process</strong>&nbsp;[WEM-9432, CVADHELP-15147]</li></ul></li><li>After you upgrade the WEM agent to version 2005,&nbsp;<strong>Citrix WEM Agent Host Service</strong>&nbsp;might consume between 10% and 30% of the total CPU resources, affecting the user experience. [WEM-9902, WEMHELP-47]</li><li>When using the application security feature, you see a green checkmark next to a user or user group in the&nbsp;<strong>Assigned</strong>&nbsp;column of the&nbsp;<strong>Assignments</strong>&nbsp;section in the&nbsp;<strong>Edit Rule</strong>&nbsp;or&nbsp;<strong>Add Rule</strong>&nbsp;window. The green checkmark icon does not necessarily indicate that the rule is assigned to that user or user group. Only a user or user group that has a blue highlight in the background is the one to which the rule is assigned. [WEM-10047]</li><li>While the WEM agent performs application processing during logon, Windows might display the&nbsp;<strong>Problem with Shortcut</strong>&nbsp;dialog box, prompting end users to delete a shortcut that no longer works properly. The issue occurs when the item to which the shortcut refers has been changed or moved. [WEM-10257, CVADHELP-15968]</li><li>When you attempt to edit an exception for an application security rule on the&nbsp;<strong>Exceptions</strong>&nbsp;tab of the&nbsp;<strong>Edit Rule</strong>&nbsp;window, the administration console might exit unexpectedly. The following message appears:<ul><li><strong>Connection to the management console closed</strong></li></ul>The issue occurs when you edit an exception before performing any&nbsp;<strong>Add Rule</strong>&nbsp;actions. [WEM-10612]</li><li>When you use&nbsp;<strong>VUEMRSAV.exe</strong>&nbsp;to view results about assigned actions for the current user, the&nbsp;<strong>Applied Actions</strong>&nbsp;tab fails to display actions related to Action Groups. (<strong>VUEMRSAV.exe</strong>&nbsp;is located in the agent installation folder:&nbsp;<code>%ProgramFiles%\Citrix\Workspace Environment Management Agent\VUEMRSAV.exe</code>.) [WEM-10889]</li></ul>
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
