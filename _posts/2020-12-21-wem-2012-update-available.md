---
layout: post
title: WEM 2012 UPDATE AVAILABLE
date: 2020-12-21
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2012-update-available/elf-covid-meme-christmas-2020.jpg
share-img: /assets/img/wem-2012-update-available/elf-covid-meme-christmas-2020.jpg
---
![It's COVID Outside](/assets/img/wem-2012-update-available/elf-covid-meme-christmas-2020.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>What a year 2020 has been. Despite all the ups and downs, Citrix continues to push out updates. They have released the last version of WEM for 2020, version 2012. You can download the new version <a rel="noreferrer noopener" href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2012.html" target="_blank">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2012">What’s new in 2012<a href="void(0)"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management 2012 includes the following new features. For information about bug fixes, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">Fixed issues</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="wem-agent-integration-with-the-citrix-virtual-apps-and-desktops-product-software">WEM agent integration with the Citrix Virtual Apps and Desktops product software</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The WEM agent is integrated with the Citrix Virtual Apps and Desktops product software, letting you include the WEM agent when installing a Virtual Delivery Agent (VDA). This integration is reflected in the Citrix Virtual Apps and Desktops 2012 product software and later. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/whats-new.html#citrix-virtual-apps-and-desktops-7-2012">Citrix Virtual Apps and Desktops 7 2012</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you enable the&nbsp;<strong>Citrix Workspace Environment Management</strong>&nbsp;check box on the&nbsp;<strong>Additional Components</strong>&nbsp;page, the&nbsp;<strong>WEM Agent</strong>&nbsp;page appears. That page is titled&nbsp;<strong>Citrix Cloud Connectors used by WEM Agent</strong>. Enter a comma-separated list of Cloud Connector FQDNs or IP addresses. Then click&nbsp;<strong>Add</strong>. The WEM agent on the VDA communicates with those Cloud Connectors.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="optimized-wem-agent-startup">Optimized WEM agent startup</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Previously, the WEM agent startup workflow had the following issues:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>The agent did not refresh the Citrix Cloud Connector settings after startup. As a result, the Cloud Connector settings deployed to the agent through group policies did not work as expected.</li><li>In a non-persistent environment, when the agent cache file resided in the base image, the agent could experience cache synchronization issues. As a result, WEM settings might not be applied properly.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Starting with this release, the agent refreshes Cloud Connector settings after startup, just like it refreshes other settings. To ensure that the agent cache is up to date, the agent automatically recreates the cache in non-persistent environments. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html#agent-startup-behaviors">Agent startup behaviors</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For information about how to make the WEM agent work optimally, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html#prerequisites-and-recommendations">Prerequisites and recommendations</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="new-agent-cache-utility-options">New agent cache utility options</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release adds the following agent cache utility options:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><code>-RefreshSettings</code>&nbsp;or&nbsp;<code>-S</code>: Refreshes agent host settings.</li><li><code>-Reinitialize</code>&nbsp;or&nbsp;<code>-I</code>: Reinitializes the agent cache when used together with the&nbsp;<code>-RefreshCache</code>&nbsp;option.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html#agent-cache-utility-options">Agent cache utility options</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="citrix-optimizer">Citrix optimizer</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Citrix optimizer now provides you with an additional option that enables WEM to automatically select templates for your OSs:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Automatically Select Templates to Use</strong>. If you are unsure which template to use, use this option to let WEM select the best match for each OS. You can also apply this option to custom templates with different name formats by using the&nbsp;<strong>Enable Automatic Selection of Templates Starting with Prefixes</strong>&nbsp;option.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/citrix-optimizer.html">Citrix optimizer</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="support-for-the-windows-10-2004-template">Support for the Windows 10 2004 template</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>WEM adds support for the Windows 10 2004 template introduced in Citrix optimizer. You can now use WEM to perform template-based system optimizations for Windows 10 2004 machines. For information about using Citrix optimizer, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/citrix-optimizer.html">Citrix optimizer</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="support-for-editing-group-policy-settings">Support for editing Group Policy settings</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Previously, you could change only the name and description for a GPO after importing your GPO settings. You can now edit registry operations associated with a GPO. You can also add new registry operations to a GPO if needed. Currently, WEM supports adding and editing only Group Policy settings that are associated with the&nbsp;<code>HKEY_LOCAL_MACHINE</code>&nbsp;and the&nbsp;<code>HKEY_CURRENT_USER</code>&nbsp;registry hives.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When editing Group Policy settings, you have the following actions:&nbsp;<strong>Set value</strong>,&nbsp;<strong>Delete value</strong>,&nbsp;<strong>Create key</strong>,&nbsp;<strong>Delete key</strong>, and&nbsp;<strong>Delete all values</strong>. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/group-policy-settings.html#edit-group-policy-settings">Edit Group Policy settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="multiple-selection-support-for-action-groups">Multiple selection support for action groups</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Previously, when adding actions to an action group, you moved each action present in the&nbsp;<strong>Available</strong>&nbsp;pane to the&nbsp;<strong>Configured</strong>&nbsp;pane one by one. You can now move multiple actions in a single step. For more information about action groups, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/action-groups.html">Action Groups</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="wem-agent-advanced-notice">WEM agent (advanced notice)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Microsoft Sync Framework 2.1 will reach End of Life on January 12, 2021. WEM will retire the associated legacy agent cache sync service and switch to using the latest agent cache sync service to keep the agent cache in sync with the infrastructure services. The latest agent cache sync service relies on&nbsp;<em>Dotmim.Sync</em>, an open-source sync framework. How does this change impact you?</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>If you use Workspace Environment Management 1912 or later, this change does not require action on your part.</li><li>If you use Workspace Environment Management 1909 or earlier, upgrade to Workspace Environment Management 1912 or later.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This change is scheduled to be rolled out in March 2021.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1>Known issues</h1>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>After you upgrade the WEM agent to version 1912, the memory consumption of <strong>Citrix WEM Agent Host Service</strong> might exceed 2G. If debug mode is enabled, you can see that the following messages appear many times in the <strong>Citrix WEM Agent Host Service Debug.log</strong> file:<ul><li><strong>Adding history entry to the DB writer queue</strong></li><li><strong>Initializing process limitation thread for process</strong> [WEM-9432, CVADHELP-15147]</li></ul></li><li>After you upgrade the WEM agent to version 2005, <strong>Citrix WEM Agent Host Service</strong> might consume between 10% and 30% of the total CPU resources, affecting the user experience. [WEM-9902, WEMHELP-47]</li><li>The WEM administration console might fail to display the changes you made to the working directory for an installed application the next time you edit the application. [WEM-10007, CVADHELP-15695]</li><li>When using the application security feature, you see a green checkmark next to a user or user group in the <strong>Assigned</strong> column of the <strong>Assignments</strong> section in the <strong>Edit Rule</strong> or <strong>Add Rule</strong> window. The green checkmark icon does not necessarily indicate that the rule is assigned to that user or user group. Only a user or user group that has a blue highlight in the background is the one to which the rule is assigned. [WEM-10047]</li><li>In non-persistent environments, changes you make through the administration console might fail to take effect on the agent hosts. The issue occurs because the agent cache file in the base image might cause cache synchronization problems. As a workaround, users need to first delete the cache on their agent hosts and then refresh the cache manually to synchronize the cache with the infrastructure services.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The recommended best practice is to use a persistent location for the agent cache. If the agent cache resides in a non-persistent location, take these steps before sealing the base image:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>Stop <strong>Citrix WEM Agent Host Service</strong>.</li><li>Delete these agent local database files: <strong>LocalAgentCache.db</strong> and <strong>LocalAgentDatabase.db</strong>. [WEM-10082]</li></ol>
<!-- /wp:list -->

<!-- wp:list -->
<ul><li>The following options are not mutually exclusive. However, the administration console does not allow you to configure them at the same time.<ul><li><strong>Hide Specified Drives from Explorer</strong> and <strong>Restrict Specified Drives from Explorer</strong> (on the <strong>Policies and Profiles &gt; Environmental Settings &gt; Windows Explorer</strong> tab) [WEM-10172, WEMHELP-52]</li></ul></li></ul>
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
