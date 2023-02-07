---
layout: post
title: Storefront - Resolving Duplicate Icons
date: 2015-06-18
readtime: true
tags: [Administration, Storefront]
thumbnail-img: /assets/img/storefront-resolving-duplicate-icons/ronburgundy.jpg
share-img: /assets/img/storefront-resolving-duplicate-icons/ronburgundy.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/storefront-resolving-duplicate-icons/ronburgundy.jpg" 
    alt="ronb">

<small><em>NOTE: This post concerns Storefront 2.6</em></small>

# Intro #
I've spent quite a bit of time branding my new Storefront site for my users. One of the persistent issues I ran into was that all my desktop and application icons were duplicated. Here is how it was resolved.

# The problem #
I had configured my Storefront servers in an HA configuration by load balancing them via NetScaler and I suspected that this was somehow duplicated the icons. After some research I found that this was not the case. Citrix Studio did not show any duplicate applications, and I was not configured for Storefront Multi-Site settings (this can result in duplicate applications as well see this <a href="http://blogs.citrix.com/2014/10/13/storefront-multi-site-settings-some-examples/"><b>Citrix article</b></a> for details on configuring this). Despite this I saw the following:

| Desktop icons | Apps List |
| :---: | :---: |
| ![desktopicons](/assets/img/storefront-resolving-duplicate-icons/dupicons1.png) | ![appslist](/assets/img/storefront-resolving-duplicate-icons/dupicons2.png) |

# The solution #
Turns out that when I configured my Storefront site, I needlessly entered 2 separate entries into the Manage Delivery Controllers section.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/storefront-resolving-duplicate-icons/mandc1.png" 
    alt="mandc1">

You are able to enter in any number of delivery controllers in one entry and Storefront will load balance between the two (or more). Ideally, if you have a NetScaler (or similar device), you can load balance your Delivery Controllers there. I Removed the Controller2 entry above and edited the Controller1 entry as below.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/storefront-resolving-duplicate-icons/mandc3.png" 
    alt="mandc3">

Now there are 2 servers listed for one entry and Storefront will load balance between them.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/storefront-resolving-duplicate-icons/mandc2.png" 
    alt="mandc2">

Done with changes. Let's log into Storefront and see what's different:

| Desktop icons | Apps List |
| :---: | :---: |
| ![desktopicons](/assets/img/storefront-resolving-duplicate-icons/dupicons3.png) | ![appslist](/assets/img/storefront-resolving-duplicate-icons/dupicons4.png) |

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*