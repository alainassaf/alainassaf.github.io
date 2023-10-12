---
layout: post
title: WEM 2203 UPDATE AVAILABLE
date: 2022-03-24
readtime: true
tags: [WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-2203-update-available/thanos.jpg
share-img: /assets/img/wem-2203-update-available/thanos.jpg
---
![Thanos](/assets/img/wem-2203-update-available/thanos.jpg)

<!-- wp:heading {"level":1} -->
<h1 id="intro">Intro</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Spring has sprung and Citrix has released a new version of WEM along with the new 2203 LTSR announcement. Citrix has released the first WEM version of 2022. You can download the new version <a href="https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/components/workspace-environment-management-2203.html">here</a> (requires Platinum/Premium licenses and login to Citrix.com). I’ve provided the release notes below. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="whats-new-in-2203">What’s new in 2203<a href="void(0)"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes the following new features and addresses&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/fixed-issues.html">issues</a>&nbsp;to improve the user experience:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="allow-users-to-self-elevate-certain-applications">Allow users to self-elevate certain applications</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release introduces self-elevation for the privilege elevation feature. With self-elevation, you can automate privilege elevation for certain users without the need to provide the exact executables beforehand. Those users can request self-elevation for any applicable file simply by right-clicking the file and then selecting&nbsp;<strong>Run with administrator privileges</strong>&nbsp;in the context menu. After that, a prompt appears, requesting that they provide a reason for the elevation. The reason is for auditing purposes. If the criteria are met, the elevation is applied, and the files run successfully with administrator privileges. In addition, self-elevation gives you flexibility to choose the best solution for your needs. You can create allow lists for files you permit users to self-elevate or block lists for files you want to prevent users from self-elevating. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/security.html#self-elevation">Self-elevation</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="configure-user-processes-as-triggers-for-external-tasks">Configure user processes as triggers for external tasks</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release includes enhancements to the external task feature. The feature now provides you with two additional options to control when to run external tasks:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li><strong>Run when processes start</strong>. Controls whether to run the external task when specified processes start.</li><li><strong>Run when processes end</strong>. Controls whether to run the external task when specified processes end.</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Using the two options, you can define external tasks to supply resources only when certain processes are running and to revoke those resources when the processes end. Using processes as triggers for external tasks lets you manage your user environments more precisely compared with processing external tasks on logon or logoff.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/actions/external-tasks.html#external-task-list">External Tasks</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="profile-container-insights">Profile container insights</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with this release, you can monitor profile containers for Profile Management and FSLogix. The feature provides insights into the basic usage data of the profile containers, the status of sessions using the profile containers, the issues detected, and more. Use the feature to stay on top of space usage for profile containers and to identify problems that prevent profile containers from working. For more information, see&nbsp;<a href="https://docs.citrix.com/en-us/workspace-environment-management/current-release/user-interface-description/monitoring.html#profile-container-insights">Profile Container Insights</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="administration-console">Administration console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The administration console user interface has changed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>In&nbsp;<strong>Security</strong>, there is a new node,&nbsp;<strong>Self-elevation</strong>. The node contains a tab that lets you automate privilege elevation for users.</li><li>In&nbsp;<strong>Monitoring</strong>, there is a new node,&nbsp;<strong>Profile Container Insights</strong>. The node contains two tabs. The&nbsp;<strong>Summary</strong>&nbsp;tab includes two pie charts, providing a summary that shows the status of profile containers. The&nbsp;<strong>Profile Container Status</strong>&nbsp;tab displays a list of status records for profile containers.</li></ul>
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
