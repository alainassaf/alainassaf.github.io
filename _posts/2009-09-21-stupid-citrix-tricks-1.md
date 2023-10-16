---
layout: post
title: Stupid Citrix Tricks
Subtitle: Part 1
date: 2009-09-21
tags: [Administration, Citrix, Remote Access]
---
We are currently working on a project to move from published applications to published desktops.  With all the logistical issues and complications to make this experience as smooth as possible, one silly annoyance cropped up.  When you log into a Citrix desktop via ICA you may see this:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 30%;"
    src="/assets/img/stupid-citrix-tricks-1/icalogin.jpg" 
    alt="ICA Login to XenApp Server">

Typically, you don't want to have your users see this because then the seemless desktop experience is a little tarnished.  There are a variety of ways to solve this problem, but the eaisest is to demonstrate your mad MSPaint skills.

On your XenApp system, look for ica256.bmp and ica.bmp under C:\Program Files\Citrix\System32.  All you have to do is open these files up and color them one solid color.  Once you do this, your users will see this:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 30%;"
    src="/assets/img/stupid-citrix-tricks-1/icaloginafter.jpg" 
    alt="ICA Login to XenApp Server modified">

Voilà, now your users hopefully won't notice that something is different.  Of course, that Windows 2003 Login may be a dead give away.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*