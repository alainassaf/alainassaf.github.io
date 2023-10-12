---
layout: post
title: Citrix Admin Duh moment
date: 2009-04-29
tags: [Citrix, Netscaler, Remote Access]
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-admin-duh-moment/Configure_Access_Control.png" 
    alt="CAC">

Since we had changed our front end to dual NetScalers (9010 Plantium edition) we ran into a problem delivering a certain application. Previously with MSAM (yes, we were one of 2 customers with that gem of a solution) we had 2 sets of servers with different session timeout settings to service internal and external users accessing the application. Users with an external IP (i.e. working from home) got routed to the external servers and vice-versa for internal.  

Due to load balancing our connections internally with 2 other Netscalers, We found that the IP forwarded to the farm would be internal, thus making the load evaluator which routed external users useless.  The only way we could find to resolve this was to move the VIP to an externally facing IP, thus our load evaluators would work and route users correctly again.

This brings me to the above picture. In all the years of managing Citrix, I've never changed the Access Control screen when publishing an application. Today, we all were looking at it and realized that it might have been a much simpler solution and would have resolved the issue quickly.  That being said, it would have required us to publish 2 different versions of the application in such a way that it would not be obvious to our users.

I just wanted to share this since we all have moments when we cannot see the forest for the trees.

Thoughts???

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*