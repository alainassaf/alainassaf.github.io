---
layout: post
title: WEM 1912 UPDATE AVAILABLE
date: 2019-12-18
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-1912-update-available/ap_dangerously.jpg
share-img: /assets/img/wem-1912-update-available/ap_dangerously.jpg
---
![ap](/assets/img/wem-1912-update-available/ap_dangerously.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Happy holidays!!! Citrix has released the last version of WEM for 2019 along with the new LTSR release for Virtual Apps/Desktops, Provisioning, StoreFront, and a partridge in a pear tree. This version is 1912. You can download the new version <a rel="noreferrer noopener" aria-label=" (opens in a new tab)" href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-1909.html" target="_blank">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-workspace-environment-management-1811">What’s new in Workspace Environment Management 1912</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 id="replacing-microsoft-sql-server-compact-sql-ce-with-sqlite">Replacing Microsoft SQL Server Compact (SQL CE) with SQLite</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Workspace Environment Management (WEM) agent can work in offline mode. In earlier releases, the agent relied on Microsoft SQL Server Compact to synchronize with SQL Server to facilitate offline mode. Microsoft SQL Server Compact 3.5 Service Pack 2 is the last version that supports this functionality. Versions 4.0 and later do not support synchronization with SQL Server. However, SQL Server Compact 3.5 Service Pack 2 reached End of Life (EOL) in 2018. Starting with this release, the agent relies on SQLite for offline mode to work.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 id="how-this-change-impacts-you">How this change impacts you</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you do not want to use Microsoft SQL Server Compact 3.5 Service Pack 2, upgrade the infrastructure services, the administration console, and the agent to the latest version. For information about upgrading these components, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/upgrade.html">Upgrade a deployment</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you continue to use Microsoft SQL Server Compact 3.5 Service Pack 2, this replacement does not require action on your part.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="support-for-exporting-and-importing-configuration-sets">Support for exporting and importing configuration sets</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with this release, WEM supports exporting and importing configuration sets using the administration console. To export configuration sets, use the&nbsp;<strong>Backup</strong>&nbsp;wizard, where the&nbsp;<strong>Configuration set</strong>&nbsp;option is available on the&nbsp;<strong>Select what to back up</strong>&nbsp;page. To import configuration sets, use the&nbsp;<strong>Restore</strong>&nbsp;wizard, where the&nbsp;<strong>Configuration set</strong>&nbsp;option is available on the&nbsp;<strong>Select what to restore</strong>&nbsp;page. You can export and import only one configuration set at a time. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/ribbon.html">Ribbon</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="option-to-reset-actions">Option to reset actions</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with this release, WEM supports resetting assigned actions (purging action-related registry entries in the user environment). The feature also provides the flexibility to reset assigned actions. You can reset all assigned actions by using the administration console or let users decide what to reset in their environment. The feature might be useful in scenarios where actions you assign to users or user groups do not take effect. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/advanced-settings.html#ui-agent-personalization">Advanced settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The administration console user interface has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>The&nbsp;<strong>Advanced Settings &gt; UI Agent Personalization &gt; UI Agent Options</strong>&nbsp;tab introduces an “Allow Users to Reset Actions” option. Use that option to control whether to let current users specify what actions to reset in their environment.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="agent-administrative-templates">Agent administrative templates</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are now two policies associated with the WEM agent cache synchronization:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Cache synchronization port</strong></li><li><strong>Cached data synchronization port</strong></li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Starting with this release, the WEM agent relies on&nbsp;<strong>Cached data synchronization port</strong>&nbsp;to keep the agent cache in sync with the WEM infrastructure service. If you have Workspace Environment Management 1909 or earlier deployed in your environment, you cannot not use&nbsp;<strong>Cached data synchronization port</strong>. Instead, use&nbsp;<strong>Cache synchronization port</strong>. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html#step-1-configure-group-policies-optional">Configure group policies</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="upgrade-enhancement">Upgrade enhancement</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release simplifies the process of upgrading the WEM database. In earlier releases, to upgrade the database, you needed to remove the database from the availability group if the database was deployed in a SQL Server Always On availability group. Starting with this release, you can upgrade the database without removing it from the availability group.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Note that you still need to back up the database before you perform the upgrade. For more information about upgrading the database, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/upgrade.html">Upgrade a deployment</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="workspace-environment-management-wem-powershell-sdk-modules">Workspace Environment Management (WEM) PowerShell SDK modules</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes enhancements to the PowerShell modules in the WEM SDK. You can now use the PowerShell SDK to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Create, update, query, and delete configuration sets&nbsp;<em>and</em>&nbsp;user-level and machine-level AD objects</li><li>Export and import configuration sets or user-level or machine-level AD objects</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="documentation">Documentation</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Workspace Environment Management documentation is updated to reflect current product behavior. The Workspace Environment Management SDK documentation is updated to version 1912.</p>
<!-- /wp:paragraph -->

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
<ul><li>Agent host machine names listed on the&nbsp;<strong>Active Directory Objects</strong>&nbsp;tab of the WEM service administration console do not update automatically to reflect changes to machine names. To display the new name of a machine in the Machines list, you must manually delete the machine from the Machines list, and then add the machine again. [WEM-1549]</li><li>Registry entries might not take effect if you assign them to a user or user group through an action group. However, they do take effect if you assign them directly. The issue occurs when you assign registry entries to be created in one of the following locations:<ul><li>%ComputerName%\HKEY_CURRENT_USER\SOFTWARE\Policies</li><li>%ComputerName%\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies [WEM-5253]</li></ul></li><li>Workspace agent refreshes might take a long time to complete. The issue occurs when the current user belongs to many user groups and there are action groups or many actions for the agent to process. [WEM-6582]</li></ul>
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
