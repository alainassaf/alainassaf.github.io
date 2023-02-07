---
layout: post
title: Known issue with EdgeSight Agent 5.3
subtitle: EdgeSight
date: 2011-03-28
tags: [Administration, Citrix, EdgeSight, Reporting, Monitoring]
---
![](/assets/img/icons/es-logo.jpg)

If you are running EdgeSight agent version 5.3 and find that some servers are not reporting to your EdgeSight server, there is a known issue with this version. See <a href="http://forums.citrix.com/message.jspa?messageID=1459520">this thread</a> in the Citrix forums and <a href="http://support.citrix.com/article/CTX125179">this CTX</a> article.

The stated cause is that your effected servers are using server-based licensing.  There are 2 workarounds available:

1. Set your server to use farm-based licensing in the Delivery Services Console / Access Management Console

2. The defect is fixed in the EdgeSight 5.3 agent Hotfix 1 (version 5.3.4132). You can download it here: <span style="text-decoration:underline;"><a href="http://support.citrix.com/product/es/xav5.3/" target="_top">http://support.citrix.com/product/es/xav5.3/</a></span> (ES530XAAgentWX64001 for x64 agents or ES530XAAgentWX86001 for x86 agents).

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*