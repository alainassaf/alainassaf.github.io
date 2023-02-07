---
layout: post
title: Disable Personalization for a User/System
subtitle: AppSense
date: 2010-11-02
tags: [AppSense, Environment Manager, Troubleshooting, User, Virtualization]
---
###### BLOG UPDATE: We recently consulted with AppSense on an unrelated issue and discussed this personalization group that disables personalization.  It was recommended that we use a condition to prevent any chance of a user/computer falling into it accidentally.  So, we used an AD Group that we can drop users or servers into as needed and applied it as a condition to this Personalization Group.  This is considered best practice by AppSense as the Default Users group should be the only one that has no conditions set.

In working with AppSense support on an application issue, they directed that I create a new personalization group that effectively disables EM personalization (application/desktop settings/customizations) for whomever/whatever is a member of it.

First, create a Personalization Group and move it to the top of list so it has the highest priority.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-disable-personalization-for-a-usersystem/image.png" width="244" height="158" alt="image">

Second, uncheck all the check boxes under the Settings tab.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-disable-personalization-for-a-usersystem/image1.png" width="644" height="278" alt="image1">

Finally, remove any applications or groups under the Whitelist tab.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-disable-personalization-for-a-usersystem/clip_image003.png" width="644" height="355" alt="clip_image003">

Now when you add a user account to the membership rules for this Personalization Group, the user will not get any application or desktop settings.  This is an ideal way to test whether your Personalization settings are causing an issue without stepping up a whole different database for testing.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*