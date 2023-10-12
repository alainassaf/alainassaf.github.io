---
layout: post
title: WEM 1906 UPDATE AVAILABLE
date: 2019-06-17
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-1906-update-available/distractedwem.jpg
share-img: /assets/img/wem-1906-update-available/distractedwem.jpg
---
![distracted](/assets/img/wem-1906-update-available/distractedwem.jpg)

<!-- wp:heading {"level":1} -->
<h1>Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Summer is just around the corner and Citrix has released another version of WEM. This version is 1906. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-1906.html">here</a> (requires Platinum licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-workspace-environment-management-1811">What’s new in Workspace Environment Management 1906</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 id="action-groups">Action Groups</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Action Groups functionality has been added to the administration console&nbsp;<strong>Actions</strong>&nbsp;pane. This functionality lets you configure a group of actions that you want to assign to a user or user group. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/action-groups.html">Action Groups</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The administration console user interface has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>In&nbsp;<strong>Actions</strong>, there is a new&nbsp;<strong>Action Groups</strong>&nbsp;pane. In the&nbsp;<strong>Actions &gt; Action Groups</strong>&nbsp;pane, there is a new&nbsp;<strong>Action Group List</strong>&nbsp;tab for configuring a group of actions that you want to assign to a user or user group.</li><li>An “Enable Notifications” option is provided on the&nbsp;<strong>Advanced Settings &gt; Configuration &gt; Agent Options</strong>&nbsp;tab. You can use this option to control whether the agent displays notification messages on the agent host when the connection to the infrastructure service is lost or restored.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="upgrade-enhancement">Upgrade enhancement</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes the following enhancements to improve the user experience with the upgrade of Workspace Environment Management:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>After upgrading the infrastructure service, the fields on all the tabs of&nbsp;<strong>WEM Infrastructure Service Configuration</strong>&nbsp;are automatically populated with the data you previously configured. As a result, you no longer need to type the required information manually after you upgrade the infrastructure service.</li><li>After upgrading the database, the fields in the&nbsp;<strong>Database Upgrade Wizard</strong>&nbsp;window are automatically populated with the data you previously configured. As a result, you no longer need to type the required information manually after you upgrade the database.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="profile-management">Profile management</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management now supports all versions of Profile Management through 1903. The following new option is now available on the&nbsp;<strong>Administration Console &gt; Policies and Profiles &gt; Citrix Profile Management Settings &gt; Synchronization</strong>&nbsp;tab:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Enable Profile Container (option for eliminating the need to save a copy of the folders to the local profile)</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 id="vmware-persona-settings-deprecation">VMware Persona settings deprecation</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Support for VMware Persona settings has been deprecated. All VMware Persona settings content will be removed from the documentation in the next release. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/deprecation.html">Deprecation</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="documentation">Documentation</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management documentation is updated to reflect current product behavior.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Fixed Issues</h3>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 id="fixed-in-workspace-environment-management--1906">Fixed in Workspace Environment Management 1906</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>Attempts to create a WEM database using the WEM Database Management Utility might fail. The issue occurs when the synchronization context creation for the WEM database times out. [WEM-3020, LD0675]</li><li>When you click&nbsp;<strong>Apply Filter</strong>&nbsp;or&nbsp;<strong>Refresh Report</strong>&nbsp;on the&nbsp;<strong>Administration Console</strong>&nbsp;&gt;&nbsp;<strong>Monitoring</strong>&nbsp;&gt;&nbsp;<strong>User Trends</strong>&nbsp;&gt;&nbsp;<strong>Devices Types</strong>&nbsp;tab, you might not be able to view the report. Instead, you are returned to the&nbsp;<strong>Administration Console</strong>&nbsp;&gt;&nbsp;<strong>Actions</strong>&nbsp;&gt;&nbsp;<strong>Applications</strong>&nbsp;&gt;&nbsp;<strong>Application List</strong>&nbsp;tab. [WEM-3254]</li><li>On Windows 10 version 1809 and Windows Server 2019, Workspace Environment Management fails to pin the applications to the task bar. [WEM-3257]</li><li>After WEM upgrades to the latest version, if you still use earlier versions of the agent, the agent fails to work properly in offline mode. This issue occurs because of the scope changes of the agent local cache file in the latest release. As a workaround, delete the old agent local cache file, and then restart the WEM Agent Host Service (Norskale Agent Host service). [WEM-3281]</li><li>On the&nbsp;<strong>Security</strong>&nbsp;tab of the administration console, if you create an AppLocker rule for a file with an .exe or a .dll extension using a file hash condition, the rule does not work. This issue occurs because WEM calculates the hash code of that file incorrectly. [WEM-3580]</li><li>On the&nbsp;<strong>Security</strong>&nbsp;tab of the administration console, if you create an AppLocker rule for a file with an .exe extension using a file path condition, the rule does not work. This issue occurs because WEM converts the path for that file incorrectly. For example, suppose you browse to the .exe file in C:\ProgramData folder. Instead of converting the file path to %OSDRIVE%\ProgramData&lt;filename&gt;, WEM converts it to %SYSTEMDRIVE%\ProgramData&lt;filename&gt;. [WEM-3581]</li><li>On the&nbsp;<strong>Security</strong>&nbsp;tab of the administration console, if you create an AppLocker rule for a file using a publisher condition, the rule does not work. This issue occurs because WEM resolves the file name incorrectly. [WEM-3582]</li><li>On the&nbsp;<strong>Security</strong>&nbsp;tab of the administration console, if you attempt to create an AppLocker rule for a file with a .bat extension, WEM fails to display the .bat file in the&nbsp;<strong>Open</strong>&nbsp;window after you browse to the folder where the .bat file is located. [WEM-3585]</li><li>The&nbsp;<strong>Enable AutoEndTasks</strong>&nbsp;option on the&nbsp;<strong>Policies and Profiles &gt; Environmental Settings &gt; SBC / HVD Tuning</strong>&nbsp;tab of the administration console does not work. [WEM-3749, LD0876]</li><li>The Application Security feature does not work on Windows servers that use non-English Windows operating systems. This issue occurs because WEM fails to start the Application Identity service in non-English language environments. [WEM-3957, LD1185]</li><li>Workspace Environment Management fails to convert a UNC path to a local path. The issue occurs when you use the&nbsp;<strong>Administration Console &gt; Actions &gt; Applications &gt; Application List</strong>&nbsp;tab to associate an icon located on the network with an application. [WEM-3977]</li><li>The Windows theme of the agent host might revert to the default if you use GPO to customize your Windows theme. The issue occurs when you enable the WEM agent to process environmental settings without selecting&nbsp;<strong>Set Specific Theme File</strong>&nbsp;and&nbsp;<strong>Set Background Color</strong>&nbsp;on the&nbsp;<strong>Policies and Profiles &gt; Environmental Settings &gt; Start Menu</strong>&nbsp;tab of the administration console. As a result, the WEM agent deletes the registry settings associated with the Windows theme. [WEM-4044, LD1246]</li><li>The Windows desktop background of the agent host might revert to the default if you use GPO to customize your Windows desktop background. The issue occurs when you enable the WEM agent to process environmental settings without selecting&nbsp;<strong>Set Specific Theme File</strong>&nbsp;and&nbsp;<strong>Set Background Color</strong>&nbsp;on the&nbsp;<strong>Policies and Profiles &gt; Environmental Settings &gt; Start Menu</strong>&nbsp;tab of the administration console. As a result, the WEM agent deletes the registry settings associated with the Windows desktop background. [WEM-4217, LD1408]</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Known Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Workspace Environment Management contains the following issues:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>On Windows Server 2012 R2, if Adobe Acrobat Reader is installed, it prevents Workspace Environment Management from associating PDF files with other PDF reader applications. Users are forced to select the PDF reader application each time they open a PDF. [WEM-33]</li><li>When you use a configuration object with Workspace Environment Management PowerShell modules SDK cmdlets, all parameters must be specified. If they are not, the command fails with an InvalidOperation error. [WEM-691, WEM-693]</li><li>In PowerShell, when you use the&nbsp;<strong>help</strong>&nbsp;command with the&nbsp;<strong>-ShowWindow</strong>&nbsp;switch to display help in a floating window for a Workspace Environment Management PowerShell cmdlet, the Examples section of the help is unpopulated. To see the examples, use the&nbsp;<strong>get-help</strong>command with the&nbsp;<strong>-examples</strong>,&nbsp;<strong>-detailed</strong>, or&nbsp;<strong>-full</strong>&nbsp;switch instead. [WEM-694]</li><li>In Transformer (kiosk) mode, and with Log Off Screen Redirection enabled, WEM might fail to redirect the user to the logon page after logging off. [WEM-3133]</li><li>When you enable the process launcher on the&nbsp;<strong>Administration Console</strong>&nbsp;&gt;&nbsp;<strong>Transformer Settings</strong>&nbsp;&gt;&nbsp;<strong>Advanced</strong>&nbsp;&gt;&nbsp;<strong>Process Launcher</strong>&nbsp;tab to launch a Windows built-in application (for example, calc.exe) as entered in the process command line field, the agent host might keep opening the application after you refresh Citrix WEM Agent. [WEM-3262]</li><li>You might experience the following two issues:<ul><li>Attempts to connect to the WEM administration console might take a few minutes to complete.</li><li>It takes a long time to load WEM configurations on agent hosts. The issue occurs when you have a large Active Directory deployment and there are many connected agents. [WEM-4225]</li></ul></li><li>Attempts to map a network drive to users might fail if the assigned drive letter is already in use by an existing network drive. The issue occurs when the existing network drive failed to connect as indicated by a red “X” on the network drive icon. [WEM-4495, LD1552]</li></ul>
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
