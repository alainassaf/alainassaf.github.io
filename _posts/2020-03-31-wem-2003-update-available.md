---
layout: post
title: WEM 2003 UPDATE AVAILABLE
date: 2020-03-31
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2003-update-available/3uuy6b.jpg
share-img: /assets/img/wem-2003-update-available/3uuy6b.jpg
---
![kitty](/assets/img/wem-2003-update-available/3uuy6b.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Spring has sprung and Citrix has released the next version of WEM. This version is 2003. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2003.html" target="_blank" rel="noreferrer noopener">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-workspace-environment-management-1811">What’s new in Workspace Environment Management 2003</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2003 includes the following new features. For information about bug fixes, see <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">Fixed issues</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="citrix-optimizer">Citrix optimizer</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Citrix optimizer is now available in Workspace Environment Management (WEM). You can use the feature to optimize user environments for better performance. Citrix optimizer runs a quick scan of user environments and then applies template-based optimization recommendations. You can optimize user environments in two ways:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>You can use built-in templates to perform optimizations. To do so, select a template applicable to the operating system.</li><li>Alternatively, you can create your own custom templates with specific optimizations you want and then add them to WEM.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/citrix-optimizer.html">Citrix optimizer</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="external-task">External task</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes enhancements to the external task feature. The feature now provides you with two additional options to control when to run external tasks:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Logoff</strong>. This option lets you specify whether to run external tasks when users log off.</li><li><strong>Reconnect</strong>. This option lets you specify whether to run external tasks when a user reconnects to a machine on which the agent is running. This option is not applicable to scenarios where the WEM agent is installed on a physical Windows device.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The logoff option can be useful in scenarios where you want to purge the user environment on logoff. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/external-tasks.html">External Tasks</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="optimized-action-processing">Optimized action processing</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with this release, WEM supports processing actions without retrieving settings from the infrastructure services. There is a new “Use Cache to Accelerate Actions Processing” option on the&nbsp;<strong>Administration Console &gt; Advanced Settings &gt; Configuration &gt; Agent Options</strong>&nbsp;tab. The option enables the WEM agent to process actions by using the agent local cache. As a result, the agent no longer needs to communicate with the infrastructure services when processing actions. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/advanced-settings.html#agent-options">Agent Options</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="optimized-logon-performance">Optimized logon performance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In earlier releases, WEM delayed user logons until the processing of user Group Policy settings completed. Starting with this release, WEM no longer delays logons, and user Group Policy settings are processed in the background by default. For information about configuring this behavior, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html#system-settings">System settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="optimized-file-type-associations">Optimized file type associations</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In previous releases, file type associations other than those for text (.txt) files did not work consistently. Starting with this release, file type associations that you configure become default associations automatically. This enhancement lets you more effectively manage user environments. In addition, you now have more flexibility in configuring file type associations. In the&nbsp;<strong>New File Association</strong>&nbsp;window, you no longer have to fill out the following fields:&nbsp;<strong>Action</strong>,&nbsp;<strong>Target application</strong>, and&nbsp;<strong>Command</strong>. You can leave the fields empty as long as you can provide the correct&nbsp;<strong>ProgID</strong>. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/file-associations.html">File Associations</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="profile-management">Profile Management</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As of this release, you can use the Workspace Environment Management to configure all settings for Citrix Profile Management 2003. The following option is now available in the administration console:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Enable multi-session write-back for FSLogix Profile Container</strong>&nbsp;(option to save changes in multi-session scenarios for FSLogix Profile Container)</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The user interface of the administration console has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>In&nbsp;<strong>System Optimization</strong>, there is a new&nbsp;<strong>Citrix Optimizer</strong>&nbsp;pane. In the pane, there is a&nbsp;<strong>Citrix Optimizer</strong>&nbsp;tab for configuring optimization-related settings.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Fixed Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 1912 contains the following fixed issues compared to Workspace Environment Management 1909:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>When you use a configuration object with Workspace Environment Management PowerShell modules SDK cmdlets, all parameters must be specified. If they are not, the command fails with an InvalidOperation error. [WEM-691, WEM-693]</li><li>In PowerShell, when you use the&nbsp;<strong>help</strong>&nbsp;command with the&nbsp;<strong>-ShowWindow</strong>&nbsp;switch to display help in a floating window for a Workspace Environment Management PowerShell cmdlet, the Examples section of the help is unpopulated. To see the examples, use the&nbsp;<strong>get-help</strong>&nbsp;command with the&nbsp;<strong>-examples</strong>,&nbsp;<strong>-detailed</strong>, or&nbsp;<strong>-full</strong>&nbsp;switch instead. [WEM-694]</li><li>In Transformer (kiosk) mode, and with Log Off Screen Redirection enabled, WEM might fail to redirect the user to the logon page after logging off. [WEM-3133]</li><li>The administration console might exit unexpectedly when you scroll down the agent list on the&nbsp;<strong>Administration Console &gt; Agents &gt; Statistics</strong>&nbsp;tab. [WEM-6004]</li><li>The&nbsp;<strong>Use Cache Even When Online</strong>&nbsp;option on the&nbsp;<strong>Administration Console &gt; Advanced Settings &gt; Configuration &gt; Agent Options</strong>&nbsp;tab might not work. [WEM-6118]</li><li>Attempts to import registry files might fail with the following error message: Error “Import from Registry file” - Import Completed with Errors. The issue occurs when a registry file to be imported contains two or more values that have the same name. [WEM-6232]</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Known Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management contains the following issues:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>On the agent host, attempts to start a published application as an application shortcut might fail. The issue occurs with application shortcuts that are created using StoreFront URLs. [WEM-7348, CVADHLEP-14061]</li><li>Attempts to start an application from the&nbsp;<strong>My Applications</strong>&nbsp;icon list in the agent UI might fail. The issue occurs with application shortcuts that are created using StoreFront URLs. [WEM-7578, CVADHLEP-14171]</li><li>When the number of connected agents exceeds a certain threshold (for example, 800), Norskale Broker Service.exe might consume a significant amount of CPU resources. For example, its CPU usage can increase to 70%. [WEM-7773]</li></ul>
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
