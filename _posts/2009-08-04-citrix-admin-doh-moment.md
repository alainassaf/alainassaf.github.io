---
layout: post
title: Citrix Admin Do'h Moment
subtitle: AMC 4.6.4
date: 2009-08-04
tags: [Administration, Citrix]
---
Last night, I upgraded a couple of our XenApp administration servers to the latest version of the **Access Management Console (v. 4.6.4)**.  

Ah, the AMC, we didn't know how good we had it with the old CMC.  Anyway, I did the upgrade to address the slowness of using this console in our environment.  Discovery and doing most functions took a long time.

So, the upgrade went without issue and in the morning my co-workers remarked that the AMC is a lot more usable.  Of course, within an hour I found the same issue that several have written about in the Citrix forums like ~~**here**~~ and ~~**here**~~.  Looking at a published application list on any of the servers resulted in stopping the IMA service on the server used for discovery.

There is a private hotfix available <a href="http://support.citrix.com/article/CTX121432" target="_blank">here</a>.  You will have to have a valid MyCitrix login to get this hotfix.  I'm trying to get it now to see if it resolves the issue.

We also found the same issue with AMC 5.0.1 for Windows Server 2008.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*