---
layout: post
title: WEM 1808 UPDATE AVAILABLE
date: 2018-10-12
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-1808-update-available/davidpumpkins.jpg
share-img: /assets/img/wem-1808-update-available/davidpumpkins.jpg
---
![davidspumkins](/assets/img/wem-1808-update-available/davidpumpkins.jpg)

<h1>Intro</h1>
Fall, my favorite time of year. More so now that Citrix has released the next version of WEM. The version numbering system in now in line with other newly released Citrix products. This version is 1808. You can now download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-1808.html" target="_blank" rel="noopener">here</a> (requires Platinum licenses and login to Citrix.com). I’ve provided the release notes below. I also have it on good authority that Citrix added Pumpkin Spice to this this version.
<h1>What’s new</h1>
<h2 id="whats-new-in-workspace-environment-management-1808">What’s new in Workspace Environment Management 1808</h2>
Workspace Environment Management 1808 includes the following new features. For information about bug fixes, see <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">Fixed issues</a>.
<h3 id="new-product-names">New product names</h3>
If you’ve been a Citrix customer or partner for a while, you’ll notice new names in our products and in this product documentation. If you’re new to this Citrix product, you might see different names for a product or component.

The new product and component names stem from the expanding Citrix portfolio and cloud strategy. Articles in this product documentation use the following names:
<ul>
	<li><strong>Citrix Virtual Apps and Desktops:</strong> Citrix Virtual Apps and Desktops offers a virtual app and desktop solution, provided as a cloud service and as an on-premises product, giving employees the freedom to work from anywhere on any device while cutting IT costs. Deliver Windows, Linux, web, and SaaS applications or full virtual desktops from any cloud: public, on premises or hybrid. Virtual Apps and Desktops was formerly XenApp and XenDesktop.</li>
	<li><strong>Citrix Workspace app:</strong> The Citrix Workspace app incorporates existing Citrix Receiver technology as well as the other Citrix Workspace client technologies. It has been enhanced to deliver additional capabilities to provide end users with a unified, contextual experience where they can interact with all the work apps, files, and devices they need to do their best work. For more information, see this <a href="https://www.citrix.com/blogs/2018/07/03/your-citrix-workspace-app-journey-begins/?_ga=2.91265683.845242646.1534122644-817132532.1530263594">blog post</a>.</li>
	<li><strong>Citrix Provisioning:</strong> The Citrix Provisioning is a solution for managing virtual machine images, combining previous technologies known as Machine Creation Services (MCS) and Citrix Provisioning Services (PVS). Citrix Provisioning was formerly Provisioning Services.</li>
</ul>
Here’s a quick recap:
<table>
<thead>
<tr>
<th>Is</th>
<th>Was</th>
</tr>
</thead>
<tbody>
<tr>
<td>Citrix Virtual Apps and Desktops</td>
<td>XenApp and XenDesktop</td>
</tr>
<tr>
<td>Citrix Workspace app</td>
<td>Citrix Receiver</td>
</tr>
<tr>
<td>Citrix Provisioning</td>
<td>Provisioning Services</td>
</tr>
</tbody>
</table>
Implementing this transition in our products and their documentation is an ongoing process.
<ul>
	<li>In-product content might still contain former names. For example, you might see instances of earlier names in console text, messages, and directory/file names.</li>
	<li>It is possible that some items (such as commands and MSIs) might continue to retain their former names to prevent breaking existing customer scripts.</li>
	<li>Related product documentation and other resources (such as videos and blog posts) that are linked from this product’s documentation might still contain former names.</li>
</ul>
Your patience during this transition is appreciated. For more detail about our new names, see <a href="https://www.citrix.com/about/citrix-product-guide/">https://www.citrix.com/about/citrix-product-guide/</a>.
<h3 id="new-product-and-component-version-numbers">New product and component version numbers</h3>
In this release, product and component version numbers are displayed in the format: <strong><em>YYMM.c.m.b</em></strong>.
<ul>
	<li><em>YYMM</em> = Year and month when the features are finalized. For example, if the features are finalized in August, a release in September 2018 appears as 1808.</li>
	<li><em>c</em> = Maintenance version (if applicable).</li>
	<li><em>m</em> = Citrix Cloud release number for the month.</li>
	<li><em>b</em> = Build number. This field is shown only on the About page of the product, and in the OS’s feature for removing or changing programs.</li>
</ul>
For example, <strong>Workspace Environment Management 1808.0.1</strong> indicates that the released product with features finalized in August 2018 is associated with Citrix Cloud release 1 in that month, and is not a maintenance version. Some UI elements display only the version’s year and month, for example, <strong>Workspace Environment Management 1808</strong>.

In earlier releases of this product (Workspace Environment Management 4.7 and earlier), version numbers were displayed in the format: 4.version, where the version value incremented by one for each release. For example, the release following 4.6 was 4.7. Those earlier releases will not be updated with the new numbering format.
<h3 id="administration-console">Administration console</h3>
In this release, an “Everyone” default group is provided on the <strong>Assignments &gt; Action Assignment</strong> tab. To simplify assigning actions for all users in Active Directory, you can use the ‘Everyone’ default group to assign the actions.
<h3 id="profile-management">Profile management</h3>
As of this release, Workspace Environment Management supports configuring all settings for Citrix Profile Management 1808. The following new options are now available in the administration console:
<ul>
	<li><strong>Enable application profiler</strong> (option for defining application-based profile handling)</li>
	<li><strong>Enable search index roaming for Microsoft Outlook users</strong> (option for improving the user experience when searching mail in Microsoft Outlook)</li>
	<li><strong>Enable Large File Handling</strong> (option for eliminating the need to synchronize large files over the network)</li>
</ul>
<h3 id="documentation">Documentation</h3>
Workspace Environment Management documentation is updated to reflect current product behavior.

The <a href="https://developer-docs.citrix.com/projects/workspace-environment-management-sdk/en/latest/">Workspace Environment Management SDK documentation</a> is updated to version 1808.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for Reading,*  
*Alain Assaf*