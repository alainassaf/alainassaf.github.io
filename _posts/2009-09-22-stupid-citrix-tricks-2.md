---
layout: post
title: Stupid Citrix Tricks
subtitle: Part 2
date: 2009-09-22
tags: [Administration, Citrix, Reporting, Monitoring]
---
While there are a variety of products in the virtualization market place, Citrix has provided alerting and monitoring with Resource Manager since the early days of MetaFrame.   Despite moving over to EdgeSight and other tools in our environments, we still utilize RM for watching certain metrics and sending e-mail alerts on them.  Recently, I found a limitation with the Monitoring Profiles present in XenApp 4.5. 

We have a number of similar servers in our environment and a distinct monitoring profile for them.  As we added and built servers we added them to this profile.  What was unusual, is that if you right-clicked on the server in the AMC and selected set monitoring the correct profile would be selected.  However, if you went to the profile itself and listed the servers, it did not show all of them.

It appears that the monitoring profile is limited to only display 30 servers at a time.  To ensure that all servers are actually being monitored and reported on, I simply duplicated the monitoring profile and assigned the remaining servers to it.  It remains to be seen if we get more data/alerts as a result of this, but then again, I'm calling this series stupid Citrix tricks.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*