---
layout: post
title: Policy Configuration, Multiple Conditions in Nodes
subtitle: AppSense
date: 2010-06-18
tags: [AppSense, Environment Manager, User, Virtualization]
---
<strong>AppSense</strong>, allows for very granular control of the user environment.  In most cases (if not all), it can completely replace login scripts and Active Directory Group Policies.  In this post I will highlight how to have multiple conditions in a node.

<strong>Example: Drive Mappings</strong>

You can map network shares manually, via a login script, or in group policy.  Typically, this triggers off group membership and results in some complex login scripts.  In AppSense, the drive mapping dialog is simple.  You choose the drive letter, the path, and which user will perform the mapping.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-policy-configuration-multiple-conditions-in-nodes/image3.png" width="644" height="462" alt="image3">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-policy-configuration-multiple-conditions-in-nodes/image4.png" width="644" height="466" alt="image4">

Once you have configured the drive mapping, you can apply a variety of conditions.  For this example, we’ll use group membership.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-policy-configuration-multiple-conditions-in-nodes/image5.png" width="644" height="260" alt="image5">

The first condition we trigger on is the explorer.exe process.  Processing continues to the 2 nested conditions which check the user’s AD group membership.  The light-blue rectangle to the left indicates that either one or the other group membership condition be true to continue (in other words, this is a boolean OR condition).  Finally, AppSense maps the user's drives.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*