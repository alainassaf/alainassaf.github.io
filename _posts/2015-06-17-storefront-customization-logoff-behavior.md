---
layout: post
title: Storefront Customization - Logoff behavior
date: 2015-06-17
readtime: true
tags: [Administration, Storefront]
thumbnail-img: /assets/img/storefront-customization-logoff-behavior/cap_obvious.jpg
share-img: /assets/img/storefront-customization-logoff-behavior/cap_obvious.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/storefront-customization-logoff-behavior/cap_obvious.jpg" 
    alt="capobvious">

<em><small>Note: This post concerns Storefront 2.6</small></em>

# Intro #
While testing 2 users logging into a desktop and having a different experience (regular user vs. admin), I was perplexed on the behavior of Storefront.

### This isn't your father's Web Interface ###
I logged in as one user and launched a desktop. When I signed out of Storefront, in order to sign in as a different user, my virtual desktop immediately disconnected. I had run across the behavior before when using Web Interface, but was hard pressed to find an equivalent setting in the Storefront GUI to fix it.

After seaching Citrix articles I found: <a href="https://support.citrix.com/article/CTX200828/how-to-enabledisable-workspace-control-in-storefront"><b>How to Enable/Disable Workspace Control in StoreFront</b></a>.
<ol>
	<li>Open web.config under C:\inetpub\wwwroot\Citrix\<em>storename</em>Web\</li>
	<li>Find the line containing:
<blockquote>&lt;workspaceControl enabled="true" autoReconnectAtLogon="true" logoffAction="disconnect" showReconnectButton="false" showDisconnectButton="false" /&gt;</blockquote>
</li>
	<li>Change the value of <em>logoffAction</em> to one of the following settings:
<ol>
	<li><strong>disconnect</strong> (default) - Disconnects user from applications and desktops</li>
	<li><strong>terminate</strong> - Shut down user's applications and desktops</li>
	<li><strong>none</strong> - leave sessions running and active after Storefront log off.</li>
</ol>
</li>
	<li>The article details other settings, namely if you set showReconnectButton and showDisconnectButton to true, you will get those options in a drop down for your users (similar to Web Interface).</li>
</ol>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*