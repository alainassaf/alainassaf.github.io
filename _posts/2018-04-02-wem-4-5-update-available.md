---
layout: post
title: WEM 4.6 UPDATE AVAILABLE
date: 2018-04-02
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-4-5-update-available/todayisnotthatday.jpg
share-img: /assets/img/wem-4-5-update-available/todayisnotthatday.jpg
---
![todayisnotthatday](/assets/img/wem-4-5-update-available/todayisnotthatday.jpg)


# Intro #
Spring has sprung and Citrix has released version 4.6 of WEM. You can now download the new version <a href="https://www.citrix.com/downloads/xenapp-and-xendesktop/components/workspace-environment-management-46.html" target="_blank" rel="noopener">here</a> (requires Platinum licenses and login to Citrix.com). I’ve provided the release notes below.
<h1>What’s new</h1>
<div class="richtext base section">

Workspace Environment Management 4.6 includes the following new features. For information about bug fixes, see <a href="http://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">Fixed issues</a>.

</div>
<div class="richtext base section">

<b>Assigned applications can include StoreFront store apps</b>

You can now assign resources published in Citrix StoreFront stores as application shortcuts in Workspace Environment Management. This allows you to configure Start menu shortcuts which Workspace Environment Management end users can use to easily access remote store resources. Agent host machines configured to use the Transformer feature show shortcuts to Citrix StoreFront store resources inside the Applications tab. Configure the StoreFront stores that Citrix Receiver connects to using a new Advanced Settings tab. Then add store resources as applications in the Add Application dialog, which contains a redesigned General settings tab. For more information see <a href="http://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/applications.html">Applications</a>.

<b>Transformer integrated with Receiver for Windows SDK</b>

Transformer is now integrated with the Citrix Receiver for Windows SDK. This allows you to make StoreFront-based assigned application actions available to Transformer kiosk users, and for Citrix Receiver pass-through authentication to be used. Only published applications which users have permission to access are displayed in the Transformer kiosk Applications tab.

<b>Note</b>: If you have previously configured Transformer for <b>Enable Autologon Mode</b> and now wish to configure users for Transformer integrated with Receiver for Windows SDK, you must clear the option <b>Enable Autologon Mode</b> (in the "Transformer Settings &gt; Advanced &gt; Logon/Logoff &amp; Power settings" tab). This allows users to log in to the Transformer client endpoint machine using their own credentials. These credentials are passed through to provide access to their assigned StoreFront-based applications.

<b>Active Directory performance</b>

The Active Directory Subsystem has been redesigned to improve performance and stability. Performance improvements are particularly noticeable when you add AD or OU objects, and dead forests or domains are detected in your environment.

<b>User interface</b>

The administration console user interface has changed:
<ul>
	<li>In <b>Advanced Settings</b> &gt; <b>Configuration</b> pane, there is a new <b>StoreFront</b> tab for configuring the StoreFront stores that Citrix Receiver connects to.</li>
	<li>In <b>Actions</b> &gt; <b>Applications</b>, the <b>Add Application</b> dialog <b>General Settings</b> tab is redesigned for adding StoreFront store resources as applications. The <b>Advanced Settings</b> tab <b>Application Type</b> option is removed.</li>
	<li>In <b>Active Directory Objects,</b> there is a new <b>Advanced</b> pane. The <b>AD Settings</b> tab contains a new option <b>Active Directory search timeout</b> for configuring how long Active Directory searches are performed before they time out. The default value is 1000 msec. We recommend that you use a timeout value of at least 500 msec to avoid timeouts before searches complete.</li>
</ul>
<b>Agent administrative templates</b>

The administrative templates provided to configure the agent have been renamed to make the filenames versionless. For more information see <a href="http://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/agent-host.html">Configure the agent</a>.

<b>Documentation</b>

Workspace Environment Management documentation is updated to reflect current product behavior.

</div>
### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*