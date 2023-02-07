---
layout: post
title: Runbooks or How to Make Documentation As Painful As Possible
date: 2014-06-20
readtime: true
tags: [Administration, Documentation, Repost, PowerShell, Scripting]
thumbnail-img: /assets/img/runbooks-or-how-to-make-documentation-as-painful-as-possible/odns_writearunbook.png
share-img: /assets/img/runbooks-or-how-to-make-documentation-as-painful-as-possible/odns_writearunbook.png
---

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/runbooks-or-how-to-make-documentation-as-painful-as-possible/odns_writearunbook.png" 
    alt="PoorBoromir">

# Intro #
I’ve begun the process of creating a Runbook from scratch to document a XenDesktop/XenApp environment that is completely new to me. Runbooks are useful…right? I certainly hope so, as I’m committing a lot of time to creating an exhaustive document. Having all your infrastructure and operations detailed in one place is probably best for knowledge transfer or training.  Unfortunately, it’s also great for outsourcing.

> **/rant on**  
>In my prior job, the company decided that it did not want to be in the “IT business” anymore. So, as many companies are doing these days they <span style="text-decoration:line-through;">fired</span> “let go” the IT staff and brought in a third-party to manage operations, infrastructure, development, what have you.
>
>When this morale-destroying decision was made, my staff were directed (by me) to work with the outsourcing company to regurgitate all their accumulated knowledge. If you’ve been spared this exercise, you are fortunate. 
>
>What follows is depressing and frustrating. Imparting detailed technical knowledge is difficult, especially when you’re having to document operations and infrastructure that were never documented before. Combine that with the fact that you are obsolescing yourself during this process, and you’ve got to wonder why you should come to work at all.
>
>After many, many meetings, all this information was put into runbooks. When I reviewed them, everything was very high-level and only did a good job of describing our environment. Very little was detailed on the operations side. If you have knowledgeable staff and workers who are familiar with your technology, then I suppose this is sufficient.  
> **/rant off**

In my case, I don’t have an operations group to rely on. So, I want to document not only infrastructure in detail, but also operations. This will assist in training, but I also want to use this Runbook to improve this environment with automation. None exists at this stage and it is a barrier to scaling out the environment to support more users. So where do we start?

## Define Infrastructure ##
First, take a 30,000 foot view of your environment. What are the major components? You’ve got a network, server infrastructure and possibly storage. Maybe start with an outline view
<ol>
	<li>Network</li>
	<li>Hypervisor</li>
	<li>Storage</li>
</ol>
This can be further refined (for example):
<ol>
	<li>Access Layer</li>
	<li>Servers
<ol>
	<li>Infrastructure</li>
	<li>XenApp</li>
	<li>XenDesktop</li>
</ol>
</li>
	<li>Applications</li>
	<li>Users</li>
	<li>…and so on</li>
</ol>

### Yeah, outlines are nice, but how do you get started? ###

I’m not going to sugar-coat it, this is the toughest thing to put together especially if you don’t have anything yet. The best way to get started is to keep expanding your outline with more and more detail. You can note procedures for changes, user management, application testing and so on. I’ve previously done a blog post on documenting <a href="http://wagthereal.com/2010/05/12/documenting-citrix-with-powershell/">Citrix Policies using PowerShell and MS Word</a>. 

I highly recommend using <a href="http://carlwebster.com/">Carl Webster’s</a> documentation scripts. This can fill in a lot of detail for you. Carl has the following scripts available (as of this writing):
<ul>
	<li>Active Directory</li>
	<li>DHCP (DHCP must from from Windows Server 2012)</li>
	<li>NetScaler</li>
	<li>PVS (5.6 – 7.1)</li>
	<li>XenApp 5 for Windows Server 2003</li>
	<li>XenApp 5 for Windows Server 2008</li>
	<li>XenApp 6</li>
	<li>XenApp 6.5</li>
	<li>XenDesktop 4</li>
</ul>

The scripts are available here: <a href="http://carlwebster.com/where-to-get-copies-of-the-documentation-scripts/">Where to Get Copies of the Various Documentation Scripts</a>

Carl is developing other documentation scripts (XenDesktop 5 for one), so keep checking his web site for updates.

### The Long Haul ###
I’m still actively working on my runbook (123 pages, with attachments almost 200) and have benefited from information I had from the previous environment administrators and earlier jobs, but I have my document always open and I’m making additions and edits to it daily.

### So What About You? ###
I hope this post has inspired you to work on a run book or at least encourage documenting more of your environment and procedures. Please leave any tips, ideas, or questions in the comments. I’d like to know what you do to document and I’d like to know if you don’t document.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*