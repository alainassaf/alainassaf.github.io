---
layout: post
title: Solarwinds Advanced Subnet Calculator
subtitle: Application woes
date: 2015-09-25
readtime: true
tags: [Administration, Application, Citrix]
thumbnail-img: /assets/img/solarwinds-advanced-subnet-calculator/bg_adv_subnet_calc_top.gif
share-img: /assets/img/solarwinds-advanced-subnet-calculator/bg_adv_subnet_calc_top.gif
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/solarwinds-advanced-subnet-calculator/bg_adv_subnet_calc_top.gif" 
    alt="docock">

Solarwinds Advanced Subnet Calculator (ASC) is a great, freely-available tool to generates lists of IP4 networks. I recently ran into an issue installing it in a Windows 7 virtual desktop. It would run fine for administrators, but regular users received errors like <a href="https://support.solarwinds.com/SuccessCenter/s/article/Error-Run-Time-error-339-Component-SWLogo-ocx-or-one-of-its-dependencies-not-correctly-registered-a-file-is-missing-or-invalid?language=en_US"><b>this</b></a> and <a href="https://support.solarwinds.com/SuccessCenter/s/article/Error-The-credentials-supplied-to-the-package-were-not-recognized?language=en_US"><b>this</b></a>.

I broke out <a href="https://technet.microsoft.com/en-us/library/bb896645.aspx?f=255&amp;MSPPError=-2147217396"><b>Process Monitor</b></a> and <a href="http://sourceforge.net/projects/regshot/"><b>Regshot</b></a> to investigate what was going on here. I found that regular users could not register the SWLogo.ocx file listed in the above link. This makes sense as I had applied reasonable security on non-admins.

I reinstalled the ASC as the local admin (i.e. non-domain administrator) and that fixed one error for users, but a new one popped up. Running Process Monitor again, I found that ASC was expecting read/write permissions on
<pre>c:\programdata\Solarwinds\VB\Banners.</pre>

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/solarwinds-advanced-subnet-calculator/asc_error.png" 
    alt="asc_error">

This folder contains a series of bitmap banner ads for Solarwinds products. Not at all unreasonable for a free tool. Once I modified permissions in the disk image to allow users to modify this folder, I was back in business.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*