---
layout: post
title: WEM 4.4 UPGRADE AVAILABLE
date: 2017-08-22
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-4-4-upgrade-available/free_upgrade.png
share-img: /assets/img/wem-4-4-upgrade-available/free_upgrade.png
---
![cybermen](/assets/img/wem-4-4-upgrade-available/free_upgrade.png)

# Intro #
Citrix's software developers are hard at work and have rolled out version 4.4 of Workspace Environment Manager. You can now download the new version <a href="https://www.citrix.com/downloads/xenapp-and-xendesktop/components/workspace-environment-management-44.html" target="_blank" rel="noopener">here</a> (requires Platinum licenses and login to Citrix.com). I’ve provided the release notes below.
<h1>What’s new</h1>
Workspace Environment Management 4.4 includes the following new features. For information about bug fixes, see Fixed issues (below).
<h2>Data analytics</h2>
From this release, the Workspace Environment Management infrastructure service sends anonymous usage data to Google Analytics. For more information, and for opt-out instructions, see <a href="http://docs.citrix.com/en-us/workspace-environment-management/current-release/install-and-configure/infrastructure-services.html" target="_blank" rel="noopener">Infrastructure services</a>.
<h2>Profile Management</h2>
From this release, Workspace Environment Management supports Citrix Profile Management 7.15. The following new options are now available in the administration console:
<ul>
	<li><strong>Enable Logon Exclusion Check</strong> (options controlling file system exclusions)</li>
	<li><strong>Enable Profile Streaming Exclusion List - Directories</strong> (option controlling user profile streaming)</li>
</ul>
<h2>Database maintenance</h2>
In the Infrastructure Services Configuration utility, the <strong>Database Maintenance</strong> tab has a new option <strong>Agent registrations retention period</strong>. This allows agent registration logs to be deleted after a set time, which reduces the size of the database. It also reduces lag in populating the Registrations tab in the administration console.
<h2>Documentation</h2>
At this release, Workspace Environment Management documentation is updated to reflect current product behavior. The documentation has also been remodeled as a single "versionless" documentation set describing the “current release.” This approach reduces duplication in the online documentation set, gives more focused search results, and is better suited to agile release processes. Associated changes include:
<ul>
	<li>A top level "current release" article contains links to previous documentation sets in PDF format only. (HTML documentation for previous releases is no longer provided.)</li>
	<li>"What's new" summarizes the new functionality at the current release, and in previous releases.</li>
	<li>A new "Reference" section gathers reference information in one location. Port information previously in the introductory article is relocated to "Reference."</li>
</ul>
<h2>Fixed issues</h2>
The following issues have been fixed since Version 4.4
<ul>
	<li>If you run the Workspace Environment Management administration console as a standard Windows user, and you attempt to start the Modelling Wizard, the wizard does not start. [#WEM-187]</li>
	<li>When you attempt to add a user group, which is in a different AD domain to the infrastructure server, as a processed group in the Citrix User Profile Management tab in the administration console, the exception *IndexOutOfRangeException is raised, and the group is not processed. #WEM-210]</li>
	<li>Links in "This PC" in Windows 10 do not reflect folder redirection, and still point to local folders. [#WEM-234]</li>
	<li>The Agent Host waits about 5 minutes before starting if Workspace Environment Management is installed on Windows version 8, or Server 2012, and a language pack is installed. [#WEM-244]</li>
	<li>If you launch or refresh a UI session agent when it is not bound to a configuration set, keyboard and mouse locks which are active during the agent refresh are not released. [#WEM-321]</li>
	<li>If you attempt to add an agent host machine to a configuration set when the agent host machine is in a different domain to the infrastructure service, the machine is not added in the administration console Active Directory Objects tab. This happens regardless of the actual AD topology involved (parent/child domains, multi-forest setups, one- or two-way trust relationships, and so on). [#WEM-326, #WEM-299]</li>
</ul>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*