---
layout: post
title: WEM 2106 UPDATE AVAILABLE
date: 2021-06-16
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2106-update-available/5diirv.jpg
share-img: /assets/img/wem-2106-update-available/5diirv.jpg
---
![Haddock](/assets/img/wem-2106-update-available/5diirv.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Spring has sprung and COVID-19 is retreating, so Citrix has a new WEM release. Citrix has released the first version 2106 of WEM. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2106.html" target="_blank" rel="noreferrer noopener">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2103">What’s new in 2106</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 id="overwrite-or-merge-application-security-rules">Overwrite or merge application security rules</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release adds two settings,&nbsp;<strong>Overwrite</strong>&nbsp;and&nbsp;<strong>Merge</strong>, to the&nbsp;<strong>Administration Console &gt; Security &gt; Application Security</strong>&nbsp;tab. The settings let you determine how the agent processes application security rules.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Select&nbsp;<strong>Overwrite</strong>&nbsp;if you want to overwrite existing rules. When selected, the rules that are processed last overwrite rules that were processed earlier. We recommend that you apply this setting only to single-session machines.</li><li>Select&nbsp;<strong>Merge</strong>&nbsp;if you want to merge rules with existing rules. When conflicts occur, the rules that are processed last overwrite rules that were processed earlier.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/security.html#application-security">Application Security</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="support-for-the-windows-10-2009-template">Support for the Windows 10 2009 template</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We added support for the Windows 10 2009 (also known as 20H2) template introduced in Citrix optimizer. You can now use Workspace Environment Management to perform template-based system optimizations for Windows 10 2009 machines. In addition, we have updated all existing templates to reflect changes introduced in the latest standalone Citrix optimizer. For information about using Citrix optimizer, see <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/citrix-optimizer.html">Citrix Optimizer</a>.<a href="void(0)"></a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Fixed Issues</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2106 contains the following fixed issues compared to Workspace Environment Management 2013:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>For logging level changes to take effect immediately, the WEM agent might access certain registry keys very frequently, thus affecting performance. [WEM-11217]</li><li>The WEM agent might become unresponsive when processing applications, failing to process them successfully. [WEM-11435, CVADHELP-16706]</li><li>With an action group assigned to multiple users or user groups, if you unassign it from a user or user group, the assignment might not work as expected. For example, you assign an action group to two user groups:&nbsp;<strong>Group A</strong>&nbsp;and&nbsp;<strong>Group B</strong>. If you unassign the action group from&nbsp;<strong>Group A</strong>, the action group is unassigned from&nbsp;<strong>Group B</strong>&nbsp;rather than&nbsp;<strong>Group A</strong>. [WEM-11459, WEMHELP-75]</li><li>You might experience performance issues such as slow logon or slow session disconnect when launching or disconnecting from published application sessions. The issue occurs with WEM agent 2005 and later. [WEM-11693]</li><li>When you configure an environment variable (<strong>Actions &gt; Environment Variables</strong>), attempts to use the&nbsp;<code>$Split(string,[splitter],index)$</code>&nbsp;dynamic token might fail. The issue occurs because the dynamic token does not support multi-line strings. [WEM-11915]</li><li>If you assign a file system operations action and update the action later, the files or folders that were previously copied to the user environment might be deleted. The issue occurs because the WEM agent reverts the assignment made earlier after you update the action. [WEM-11924, CVADHELP-16916]</li><li>With&nbsp;<strong>Agent Type</strong>&nbsp;set to&nbsp;<strong>CMD</strong>&nbsp;on the&nbsp;<strong>Advanced Settings &gt; Configuration &gt; Main Configuration</strong>&nbsp;tab, the&nbsp;<strong>Monitoring &gt; Daily Reports &gt; Daily Login Report</strong>&nbsp;tab might fail to display a summary of logon times across all users connected to the current configuration set. [WEM-12226]</li><li>The memory optimization feature might fail to work on WEM version 2012 and 2103. [WEM-12913, CVADHELP-17426]</li></ul>
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
