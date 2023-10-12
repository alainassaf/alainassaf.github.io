---
layout: post
title: WEM 2206 UPDATE AVAILABLE
date: 2022-06-29
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2206-update-available/5476092.jpg
share-img: /assets/img/wem-2206-update-available/5476092.jpg
---
![Happy Kid](/assets/img/wem-2206-update-available/5476092.jpg)

<!-- wp:heading {"level":1} -->
<h1 id="intro">Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As we ease into Summer, Citrix has released the new version of WEM - 2206. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2206.html">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2206">What’s new in 2206<a href="void(0)"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes the following new features and addresses&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">issues</a>&nbsp;to improve the user experience:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="process-hierarchy-control">Process hierarchy control</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release introduces the process hierarchy control feature. The feature lets you control whether certain child processes can be started through their parent processes. You create a rule by defining parent processes and then designating an allow list or a block list for their child processes. You then assign the rule on a per user or per user group basis. The following rule types are available:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Path</strong>. Applies the rule to an executable according to the executable file path.</li><li><strong>Publisher</strong>. Applies the rule according to publisher information.</li><li><strong>Hash</strong>. Applies the rule to identical executables as specified.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For the feature to work, you need to use the&nbsp;<strong>AppInfoViewer</strong>&nbsp;tool on each agent machine to enable the feature. Every time you use the tool to enable or disable the feature, a machine restart is required. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/security.html#process-hierarchy-control">Process Hierarchy Control</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="enhancements-to-memory-management">Enhancements to memory management</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes enhancements to the memory management feature. The feature now provides you with two extra options to perform memory management:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Do Not Optimize When Total Available Memory Exceeds (MB)</strong>. This option lets you specify a threshold below which WEM optimizes memory usage for idle applications.</li><li><strong>Enable Memory Usage Limit for Specific Processes</strong>. This option lets you limit the memory usage of processes by setting upper limits for the memory they can consume.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/memory-management.html">Memory Management</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The user interface of the administration console has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>A new node,&nbsp;<strong>Process Hierarchy Control</strong>, is now available in&nbsp;<strong>Security</strong>. The node contains a tab that lets you control whether certain child processes can be started through their parent processes.</li><li>An option,&nbsp;<strong>Do Not Optimize When Total Available Memory Exceeds</strong>, is now available in&nbsp;<strong>System Optimization &gt; Memory Management &gt; Memory Management</strong>. The option lets you specify a threshold limit below which Workspace Environment Management optimizes memory usage for idle applications.</li><li>A new tab,&nbsp;<strong>Memory Usage Limit</strong>, is now available in&nbsp;<strong>System Optimization &gt; Memory Management</strong>. The tab lets you configure memory usage limits for specific processes.</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 id="deprecation">Deprecation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"normal"} -->
<p class="has-normal-font-size">Click here to see <a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/deprecation.html" target="_blank" rel="noreferrer noopener">deprecated features</a>.</p>
<!-- /wp:paragraph -->

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

<!-- wp:paragraph -->
<p><em>Thanks for reading,<br />
Alain</em></p>
<!-- /wp:paragraph -->
