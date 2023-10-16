---
layout: post
title: Dueling Personalization Servers
subtitle: AppSense
date: 2010-05-19
tags: [AppSense, Environment Manager, Management Server, Reporting, Monitoring, User, Virtualization]
---
AppSense user personalization is one of the more powerful parts of the AppSense suite.  It allows a user’s customizations (desktop and application) to move with the user to their pc, laptop, or virtual desktop.  The AppSense Environment Manager  handles this function.  We are stepping up a production AppSense environment and I ran into a conflict between the production and lab AppSense environments.  In this blog post we’ll look at Auditing and Sites in the AppSense consoles to control where the user’s custom settings go.

<strong>Auditing</strong>

In the <strong>AppSense Management Console</strong>, you can activate auditing for each Deployment Group.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-dueling-personalization-servers/image2.png" width="644" height="238" alt="image2">

Each section, which obviously governs a different aspect of the AppSense suite, contains many events that report on the various functions/actions of AppSense.  Here’s the Environment Manager list of events:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-dueling-personalization-servers/image3.png" width="565" height="772" alt="image3">

Toggling the check boxes allows you to view events in the console (surprisingly, under the Events section).

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-dueling-personalization-servers/image4.png" width="386" height="484" alt="image4">

You can modify how this information is logged in the <strong>Environment Manager console</strong>.

When you’re looking at the Policy Configuration and click on the Auditing button you’ll get the following:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-dueling-personalization-servers/image5.png" width="644" height="471" alt="image5">

These settings govern how the events appear on the local device that has the installed agent.  Here, you can choose which log the events are written to , whether to make them anonymous, and even the log format.  You can also pick which events locally logged on the client device.

<em>NOTE: Auditing should only be used for troubleshooting.  Leaving this on will impact the performance of AppSense.</em>

<strong>Sites</strong>

Now that you are logging these events you can see if your configuration is recording them.  In my situation, I was seeing these events, but when I ran the Personalization Analysis I did not get any user or application data.  I found that when I connected to our lab personalization server which was setup first, I could see the data I was expecting.  This was due to the Default Users and Default Sites settings present in the lab personalization server configuration and that this original configuration was imported into the new environment.  This required us to change the configuration in both the old and new configuration to explicitly list with servers or Group Policy OU they resided in.

In my new configuration I created a new Site and named it Migration.  I added my new EM server and also added a Computer OU membership that included all my servers I wanted to discover applications from.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/appsense-dueling-personalization-servers/image6.png" width="644" height="222" alt="image6">

To make sure the old configuration did not continue to gather data from my servers, I added a new site (called Migration for consistency sake), added my new EM server, and added the same computer OU membership to it.

Now when I run Personalization Analysis on my production Personalization groups, I can see user and application data.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*