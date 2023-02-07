---
layout: post
title: WEM 1909 UPDATE AVAILABLE
date: 2019-09-19
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-1909-update-available/3b0l4s.jpg
share-img: /assets/img/wem-1909-update-available/3b0l4s.jpg
---
![Morpheus](/assets/img/wem-1909-update-available/3b0l4s.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>With every changing season, Citrix releases another version of WEM. This version is 1909. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-1909.html">here</a> (requires Platinum licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-workspace-environment-management-1811">What’s new in Workspace Environment Management 1909</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 id="workspace-environment-management-agent-installer">Workspace Environment Management agent installer</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release provides a new, unified Workspace Environment Management (WEM) agent installer. The installer bundles the WEM on-premises and service agents into a single executable. It eliminates the need to configure the ADMX templates and to edit group policies. You can choose to install the agent interactively or use the command line. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html">Agent</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The new WEM agent installer also introduces name and installation path changes. The changes include:</p>
<!-- /wp:paragraph -->

<!-- wp:group -->
<div class="wp-block-group"><div class="wp-block-group__inner-container"><!-- wp:table {"backgroundColor":"subtle-pale-green","align":"full","className":"is-style-stripes"} -->
<figure class="wp-block-table alignfull is-style-stripes"><table class="has-subtle-pale-green-background-color has-background"><tbody><tr><td><strong>IS</strong></td><td><strong>Was</strong></td><td><strong>What</strong></td></tr><tr><td> Citrix WEM Agent Host Service</td><td>Norskale Agent Host Service </td><td>Display name of the WEM agent service (WemAgentSvc) that appears in Windows Services. </td></tr><tr><td> WEM Agent Service </td><td>Norskale Agent Service</td><td>Log name that appears in the Windows Event Log.</td></tr><tr><td>Citrix.Wem.Agent.Service.exe</td><td>Norskale Agent Host Service.exe</td><td>File name that appears in the agent installation folder.</td></tr><tr><td>%ProgramFiles%\Citrix\Workspace Environment Management Agent</td><td>%ProgramFiles%\Norskale\Norskale Agent Host</td><td>Default agent installation path. This change applies only to the default installation path when you perform fresh installation. In case of in-place upgrades, the installation path remains unchanged. </td></tr></tbody></table></figure>
<!-- /wp:table --></div></div>
<!-- /wp:group -->

<!-- wp:heading -->
<h2 id="agent-switch"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#agent-switch"></a>Agent switch</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release introduces the agent switch functionality that lets you switch from the on-premises agent to the service agent. The functionality can be useful in migration use cases where you want to migrate your existing on-premises deployment to the WEM service. It simplifies and expedites the migration process. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/advanced-settings.html#agent-switch">Agent switch</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="infrastructure-service"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#infrastructure-service"></a>Infrastructure service</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes the following enhancements to the infrastructure service to improve the user experience:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>The Active Directory (AD) subsystem has been redesigned to improve performance. It can now dynamically blacklist detected dead forests. Dead forests remain in the blacklist unless they are no longer dead two hours later. The infrastructure service does not search for AD objects in forests that are in the blacklist. As a result, some OUs in the blacklisted forests might be missing in the&nbsp;<strong>Organizational Units</strong>&nbsp;window when you click&nbsp;<strong>Add OU</strong>&nbsp;from the administration console.</li><li>The infrastructure service can now cache data related to the configuration set for the agents. As a result, the infrastructure service retrieves data from AD less frequently. This also reduces the time it takes for the agents to retrieve the information from the infrastructure service. Time reduction is particularly noticeable when there are many connected agents.</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 id="infrastructure-service-configuration"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#infrastructure-service-configuration"></a>Infrastructure service configuration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release introduces the “Enable performance tuning” option on the&nbsp;<strong>Advanced Settings</strong>&nbsp;tab of the&nbsp;<strong>WEM Infrastructure Service Configuration</strong>&nbsp;utility. You can now optimize the performance in scenarios where the number of connected agents exceeds a certain threshold (by default, 200). As a result, it takes shorter time for the agent or the administration console to connect to the infrastructure service. This feature is especially useful when the agent or the administration console intermittently disconnects from the infrastructure service. In this scenario, you can set the minimum number of worker threads and asynchronous I/O threads to a greater value. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/infrastructure-services.html#configure-the-infrastructure-service">Configure the infrastructure service</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="profile-management"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#profile-management"></a>Profile Management</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management now supports all versions of Profile Management through 1909. The following new options are now available on the tabs in the&nbsp;<strong>Administration Console &gt; Policies and Profiles &gt; Citrix Profile Management Settings</strong>&nbsp;pane:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Migrate user store</strong>. Available on the&nbsp;<strong>Main Citrix Profile Management Settings</strong>&nbsp;tab, this option lets you migrate your user store without losing any data.</li><li><strong>Automatic migration of existing application profiles</strong>. Available on the&nbsp;<strong>Profile Handling</strong>&nbsp;tab, this option lets you automatically migrate existing application profiles.</li><li><strong>Outlook search index database – backup and restore</strong>. Available on the&nbsp;<strong>Advanced Settings</strong>&nbsp;tab, this option ensures the stability of the Enable search index roaming for Outlook feature.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/policies-and-profiles/citrix-upm-settings.html">Citrix Profile Management Settings</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="administration-console"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#administration-console"></a>Administration console</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The user interface of the administration console has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>An&nbsp;<strong>Agent Switch</strong>&nbsp;tab is provided in the&nbsp;<strong>Advanced Settings &gt; Configuration</strong>&nbsp;pane. The tab lets you switch from the on-premises agent to the service agent. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/advanced-settings.html#agent-switch">Agent switch</a>.</li><li>An “Auto Prevent CPU Spikes” option is provided on the&nbsp;<strong>System Optimization &gt; CPU Management &gt; CPU Management Settings</strong>&nbsp;tab. You can use this option to automatically reduce the CPU priority of processes that overload your CPU. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/cpu-management.html">CPU Management</a>.</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 id="support-for-migrating-group-policy-objects-gpos"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#support-for-migrating-group-policy-objects-gpos"></a>Support for migrating Group Policy Objects (GPOs)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with this release, you can migrate a zip backup of your GPOs to Workspace Environment Management (WEM) service. To do so, click&nbsp;<strong>Migrate</strong>&nbsp;in the ribbon of the WEM administration console. The&nbsp;<strong>Migrate</strong>&nbsp;wizard provides the flexibility to migrate your GPOs. You can select&nbsp;<strong>Overwrite</strong>&nbsp;mode or&nbsp;<strong>Convert</strong>&nbsp;mode for your migration. The&nbsp;<strong>Overwrite</strong>&nbsp;mode overwrites existing WEM settings (GPOs) when there are conflicts. The&nbsp;<strong>Convert</strong>&nbsp;mode converts your GPOs to XML files. Then you can manually import the XML files to WEM using the&nbsp;<strong>Restore</strong>&nbsp;wizard. Doing so gives you granular control over settings to be imported. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/ribbon.html">Ribbon</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="support-for-exporting-and-importing-active-directory-ad-objects"><a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/whats-new.html#support-for-exporting-and-importing-active-directory-ad-objects"></a>Support for exporting and importing Active Directory (AD) objects</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As of this release, Workspace Environment Management service adds support for exporting and importing AD objects using the administration console. To export AD objects, use the&nbsp;<strong>Backup</strong>&nbsp;wizard, where the&nbsp;<strong>Active Directory (AD) objects</strong>&nbsp;option is provided on the&nbsp;<strong>Select what to back up</strong>&nbsp;page. To import AD objects, use the&nbsp;<strong>Restore</strong>&nbsp;wizard, where the&nbsp;<strong>Active Directory (AD) objects</strong>&nbsp;option is provided on the&nbsp;<strong>Select what to restore</strong>&nbsp;page. You can specify which type of AD objects to back up and restore. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/ribbon.html">Ribbon</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Fixed Issues</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>Attempts to connect to the WEM administration console might take a long time to complete. The issue occurs when domains to which Active Directory objects (for example, groups and OUs) belong are no longer available. [WEM-3103, LD0725]</li><li>When you enable the process launcher on the&nbsp;<strong>Administration Console</strong>&nbsp;&gt;&nbsp;<strong>Transformer Settings</strong>&nbsp;&gt;&nbsp;<strong>Advanced</strong>&nbsp;&gt;&nbsp;<strong>Process Launcher</strong>&nbsp;tab to launch a Windows built-in application (for example, calc.exe) as entered in the process command line field, the agent host might keep opening the application after you refresh Citrix WEM Agent. [WEM-3262]</li><li>You might experience the following two issues:<ul><li>Attempts to connect to the WEM administration console might take a few minutes to complete.</li><li>It takes a long time to load WEM configurations on agent hosts. The issue occurs when you have a large Active Directory deployment and there are many connected agents. [WEM-4225]</li></ul></li><li>Attempts to map a network drive to users might fail if the assigned drive letter is already in use by an existing network drive. The issue occurs when the existing network drive failed to connect as indicated by a red “X” on the network drive icon. [WEM-4495, LD1552]</li><li>The agent splash screen can persist for a long time when there is a large amount of data associated with user statistics. [WEM-4674, LD1167]</li><li>The Workspace Environment Management administration console might unexpectedly exit if you edit the application security rules. The issue occurs when there is a long list of users and user groups present in the&nbsp;<strong>Edit Rule</strong>&nbsp;window and you scroll down to view them. [WEM-4960, LD1818]</li><li>You might find that there are two&nbsp;<strong>Citrix Components</strong>&nbsp;nodes in the left pane of the Group Policy Management Console. [WEM-5012]</li><li>When you log on to a Workspace Environment Management agent machine, the logon process might take longer to complete. The issue occurs because the Workspace Environment Management agent logon service (Citrix.Wem.Agent.LogonService.exe) delays the logon process for several seconds even though the Endpoint Management group policy processing is disabled. [WEM-5237]</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Known Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management contains the following issues:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>When you use a configuration object with Workspace Environment Management PowerShell modules SDK cmdlets, all parameters must be specified. If they are not, the command fails with an InvalidOperation error. [WEM-691, WEM-693]</li><li>In PowerShell, when you use the&nbsp;<strong>help</strong>&nbsp;command with the&nbsp;<strong>-ShowWindow</strong>&nbsp;switch to display help in a floating window for a Workspace Environment Management PowerShell cmdlet, the Examples section of the help is unpopulated. To see the examples, use the&nbsp;<strong>get-help</strong>&nbsp;command with the&nbsp;<strong>-examples</strong>,&nbsp;<strong>-detailed</strong>, or&nbsp;<strong>-full</strong>&nbsp;switch instead. [WEM-694]</li><li>In Transformer (kiosk) mode, and with Log Off Screen Redirection enabled, WEM might fail to redirect the user to the logon page after logging off. [WEM-3133]</li><li>Registry entries might not take effect if you assign them to a user or user group through an action group. However, they do take effect if you assign them directly. The issue occurs when you assign registry entries to be created in one of the following locations:<ul><li>%ComputerName%\HKEY_CURRENT_USER\SOFTWARE\Policies</li><li>%ComputerName%\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies [WEM-5253]</li></ul></li></ul>
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
