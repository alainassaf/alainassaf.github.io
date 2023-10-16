---
layout: post
title: WEM 4.3 Upgrade Available
date: 2017-06-06
author: wagthereal
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-4-3-upgrade-available/63426576.jpg
share-img: /assets/img/wem-4-3-upgrade-available/63426576.jpg
---
![drevil](/assets/img/wem-4-3-upgrade-available/63426576.jpg)

# Intro #
As is often the case, Citrix incremented almost all the versions of their products during the Citrix Synergy conference. Included with the new release of XenApp/XenDesktop 7.14 was WEM version 4.3. You can now download the new version <a href="https://www.citrix.com/downloads/xenapp-and-xendesktop/edition-software/platinum-714.html">here</a> (requires Platinum licenses and login to Citrix.com). I've provided the release notes below.
<h1>What's new</h1>
<h2>Site management</h2>
In previous releases, site settings were stored on the agent side and it was possible to change them from the agent GPO. Workspace Environment Management 4.3 introduces a different approach to site management which improves product security. Sites are now assigned to machines (or Security Groups or OUs) by the infrastructure service (broker) using a new Machines page in the administration console. A new Registrations tab under Administration&gt;Agents in the administration console indicates machines which are bound incorrectly to multiple sites, so that you can take the appropriate action to remove the duplicate binding. A new Registrations tab on the Agents page shows agent registration information.

From this release, Workspace Environment Management "sites" are referred to as "configuration sets" in the user interface and documentation.
<h2>Agent localization improvements</h2>
The session agent user interface is now localized for the following languages: German, Spanish, French, Italian, Japanese, Korean, Dutch, Russian, Traditional and Simplified Chinese.
<h2>User interface improvements</h2>
Various text labels and messages in the installation wizards, administration console, and GPO templates have been rationalised and made mutually consistent to improve the user experience. For example, fields used to enter the same parameters in different installation wizards now use the same labels. Current and changed terminology is describe in a new glossary.
<h2>Documentation</h2>
Workspace Environment Management 4.3 documentation is updated to reflect current product behaviour. Various minor improvements have also been made, including the following improvements designed to assist users:
<ul>
	<li>a number of installation field descriptions have been revised to better explain their purpose</li>
	<li>the documentation uses new standardized terminology visible in the installation wizards, GPO templates, and in the administration console. For example, the term "broker" is replaced by "infrastructure service".</li>
	<li>a glossary has been added to explain the new terminology seen in the installation wizards, the administration console, and the documentation. Changed terms are also indicated.</li>
	<li>the technical overview diagram is updated</li>
	<li>a new port information table has been added to summarize port usage</li>
</ul>
<h2>Fixed issues</h2>
The following issues have been fixed since Version 4.3:
<ul>
	<li>When the Workspace Environment Management session agent is running in command line mode, User Statistics data is not reported to the WEM infrastructure services. [WEM-41]</li>
	<li>The Workspace Environment Management session agent interface does not render correctly when a computer display is extended to external displays connected via a dock. This problem, which occurs when extending to multiple displays with different screen resolution settings, results in a portion of the right-hand side of the display not rendering completely. This prevents users seeing the home button or being able to change other native Workspace Environment Management settings. [WEM-90]</li>
	<li>The Workspace Environment Management session agent causes the mouse to stop working on virtual machines which have the System Center Configuration Manager (SCCM) client installed with Power Management enabled. [WEM-115]</li>
	<li>When you are using the Transformer feature, the session agent generates an unhandled exception if Wi-Fi is turned off using "ms-settings:network-wifi." [WEM-133]</li>
	<li>The Workspace Environment Management session agent causes the mouse to stop working on virtual machines after an interruption to network access is restored. [WEM-159]</li>
</ul>
<h2>Known issues</h2>
This release contains the following issues:
<ul>
	<li>On Windows Server 2012 R2, if Adobe Acrobat Reader is installed it prevents Workspace Environment Management associating files of type .PDF with other PDF reader applications. Users are forced to manually select the PDF reader application to use each time they open a PDF. [#WEM-33]</li>
</ul>
<h2>Deprecated</h2>
<table class="citrix-table fixedtable">
<thead class="heading">
<tr>
<th>Item</th>
<th>Announced in</th>
<th>Alternative</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<div>

Support for assigning and binding existing (pre-version 4.3) agents to sites via GPO.

</div></td>
<td>
<div>

4.3

</div></td>
<td>
<div>

Upgrade agents to Workspace Environment Management 4.3.

</div></td>
</tr>
<tr>
<td>
<div>

The Administration Console will not be supported on the following platforms after the next LTSR:

Windows XP SP3 32-bit and 64-bit
Windows Vista SP1 32-bit and 64-bit
Windows 8.x 32-bit and 64-bit
Windows Server 2003 32-bit and 64-bit
Windows Server 2003 R2 32-bit and 64-bit Windows Server 2008
Windows Server 2008 R2

</div></td>
<td>
<div>

4.2

</div></td>
<td></td>
</tr>
<tr>
<td>
<div>

Workspace Environment Management will not be supported on the following software after the next LTSR:

Microsoft .NET Framework 4.0
Microsoft .NET Framework 4.5.0
Microsoft .NET Framework 4.5.1

</div></td>
<td>
<div>

4.2

</div></td>
<td></td>
</tr>
</tbody>
</table>
<h2>Removed</h2>
The following platforms, Citrix products, and features are either removed in Workspace Environment Management 4.3 or are no longer supported in Workspace Environment Management 4.3.
<table class="citrix-table datatable">
<thead class="heading">
<tr>
<th>Item</th>
<th>Replacement</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<div>

Support for assigning and binding version 4.3 agents to sites via GPO.

</div></td>
<td>
<div>

Assign and bind version 4.3 agents to sites via administration console.

</div></td>
</tr>
</tbody>
</table>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*