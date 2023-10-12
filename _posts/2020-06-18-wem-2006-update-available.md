---
layout: post
title: WEM 2006 UPDATE AVAILABLE
date: 2020-06-18
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2006-update-available/45jfxa.jpg
share-img: /assets/img/wem-2006-update-available/45jfxa.jpg
---
![Scruffy](/assets/img/wem-2006-update-available/45jfxa.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Despite stay at home orders, the Citrix update cycle never stops. Citrix has released the next version of WEM. This version is 2006. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2006.html" target="_blank" rel="noreferrer noopener">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-workspace-environment-management-1811">What’s new in Workspace Environment Management 2006</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2006 includes the following new features. For information about bug fixes, see below.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="enhancements-to-group-policy-object-gpo-migration">Enhancements to Group Policy Object (GPO) migration</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release makes further enhancements to GPO migration. Different from the&nbsp;<strong>Migrate</strong>&nbsp;wizard, which lets you migrate only Group Policy Preferences (GPP), you can now also import Group Policy settings (registry-based settings) into WEM. After importing the settings, you can have an itemized view of the settings associated with each GPO before you decide which GPO to assign. You can assign the GPO to different users or user groups. To import Group Policy settings, navigate to&nbsp;<strong>Administration Console &gt; Actions &gt; Group Policy Settings</strong>, select&nbsp;<strong>Enable Group Policy Settings Processing</strong>, and then click&nbsp;<strong>Import</strong>&nbsp;to open the import wizard. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/group-policy-settings.html">Group Policy Settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The administration console user interface has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>In&nbsp;<strong>Actions</strong>, there is a new&nbsp;<strong>Group Policy Settings</strong>&nbsp;pane. In the pane, there is a&nbsp;<strong>Group Policy Settings</strong>&nbsp;tab for you to configure Group Policy settings.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Fixed Issues</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>The WEM agent in an OU might fail to retrieve its configuration set from the infrastructure service. As a result, the agent fails to register with the associated configuration set. The issue can occur if there are multiple OUs that are present in the administration console and one or more OUs cannot be accessed. [WEM-6214, CVADHELP-13442]</li><li>On the agent host, attempts to start a published application as an application shortcut might fail. The issue occurs with application shortcuts that are created using StoreFront URLs. [WEM-7348, CVADHLEP-14061]</li><li>Attempts to start an application from the&nbsp;<strong>My Applications</strong>&nbsp;icon list in the agent UI might fail. The issue occurs with application shortcuts that are created using StoreFront URLs. [WEM-7578, CVADHELP-14171]</li><li>When the number of connected agents exceeds a certain threshold (for example, 800), Norskale Broker Service.exe might consume a significant amount of CPU resources. For example, its CPU usage can increase to 70%. [WEM-7773]</li><li>When the number of connected agents exceeds a certain threshold, Norskale Broker Service.exe might consume a significant amount of CPU resources. [WEM-7886, CVADHELP-14272]</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Known Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management contains the following issues:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>No new issues have been observed in this release.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Depreciated Features</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Click <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/deprecation.html">here</a> to see depreciated features.</p>
<!-- /wp:paragraph -->

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

<!-- wp:paragraph -->
<p><em>Thanks for reading,<br />
Alain</em></p>
<!-- /wp:paragraph -->
