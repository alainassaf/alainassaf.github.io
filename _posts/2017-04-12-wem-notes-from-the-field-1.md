---
layout: post
title: WEM - Notes From the Field 1
date: 2017-04-12
readtime: true
tags: [Learning, Norskale, WEM, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-notes-from-the-field-1/picard_wem.jpg
share-img: /assets/img/wem-notes-from-the-field-1/picard_wem.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/wem-notes-from-the-field-1/picard_wem.jpg" 
    alt="picard">

# Intro #
In <a href="https://www.citrix.com/blogs/2016/09/08/citrix-acquires-norskale-making-the-industrys-best-app-desktop-delivery-performance-even-better/" target="_blank">September 2016</a>, Citrix announced the purchase of <a href="http://norskale.com/" target="_blank"><b>Norskale</b></a>. Now dubbed Workspace Environment Manager (rolls off the tongue 😀), Citrix has a powerful tool that provides system optimization along with user environment control. I'm implementing a small VDI solution utilizing this new tool and these notes are primarily for me and others to note some particulars of this product.

# Registry #
They always say RTFM, but I was pressed for time and only dived into the installation guide for WEM 4.2. While attempting to apply settings to the registry I became more and more frustrated because nothing was happening. I finally went back to the Citrix Docs and found out my issue.

When you apply registry settings in WEM they ONLY affect Current User Registry entries:
<blockquote>Target Path. The registry location in which the registry entry will be created. Citrix Workspace Environment Management can only create Current User registry entries, so you do not need to preface your value with %ComputerName%\HKEY_CURRENT_USER\ this is done automatically.</blockquote>
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/wem-notes-from-the-field-1/wem11.png" 
    alt="wem11">

# WEM Sites #
You can create any number of WEM sites to manage your environment. They can be OS specific, user specific or just separate to test changes in an acceptance environment before making the changes in production.

In the home tab of the WEM Console, there are import export buttons

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/wem-notes-from-the-field-1/wem21.png" 
    alt="wem21">

Exporting actions will allow you to export any settings you have under the Action pane

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/wem-notes-from-the-field-1/wem3.png" 
    alt="wem3">

Exporting Settings will export settings under the System Optimization, Policies and Profiles, Transformer Settings, and Advanced Settings panes

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/wem-notes-from-the-field-1/wem4.png" 
    alt="wem4">

Now here's the rub. The following items are not exported:
<ul>
	<li>Filters</li>
	<li>Assignments</li>
	<li>Configured Users</li>
	<li>Administration</li>
</ul>
You will have to redo these settings yourself. Luckily, you can easily switch sites can compare settings

That's all for now. More to come as I dive deeper into WEM.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*