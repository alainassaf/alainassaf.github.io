---
layout: post
title: Servers Not Appearing in Console
subtitle: EdgeSight
date: 2011-04-21
tags: [Administration, Citrix, EdgeSight, Troubleshooting]
---
<img style="background-image:none;padding-left:0;padding-right:0;display:inline;float:left;padding-top:0;border:0;margin:0 15px 0 5px;" title="index" src="/assets/img/icons/es-logo.jpg" alt="" width="134" height="134" align="left" border="0" />
Recently we started migrating users to our new production environment which is provisioned.  I found that only half of the new servers were reporting to EdgeSight.  Initially I thought it was due to an issue solved by this [**EdgeSight agent hotfix**](https://support.citrix.com/article/CTX129053/hotfix-es530xa6agentwx64004-version-5341363-for-citrix-edgesight-53-xenapp6-agent-x64), but this turned out to not be the case.

Digging deeper, I found that the missing servers had the same server name in the `\Citrix\SystemMonitoring\Data\EdgeSight.ini` file.  The quick fix was to stop the EdgeSight service (Citrix System Monitoring), delete the INI file, and restart the service.  I then forced a Configuration Check and Performance Upload on the worker agents and the servers appeared in the console.

Since these were provisioned servers and the EdgeSight data writes to a “cache” drive, the long-term fix is to mount the “cache” drive associated with our template and delete the INI file, then new servers will get a fresh INI file.

<strong>Here's a list of EdgeSight Troubleshooting articles from Citrix</strong>

<a href="http://support.citrix.com/article/CTX111043" target="_blank">CTX111043</a>: Newly Installed EdgeSight Agent Devices Do Not Report Up

<a href="http://support.citrix.com/article/CTX114939" target="_blank">CTX114939</a>: Troubleshooting EdgeSight

<a href="http://support.citrix.com/article/CTX123446" target="_blank">CTX123446</a>: Real Time Remote Report Error: Access denied: You do not have permission to access this resource

<a href="http://support.citrix.com/article/CTX123293" target="_blank">CTX123293</a>: EdgeSight Remote, Troubleshoot, and Real-time Reports Error Messages

<a href="http://support.citrix.com/article/CTX118565" target="_blank">CTX118565</a>: EdgeSight 5.0 Frequently Asked Questions

<a href="http://support.citrix.com/article/CTX115712" target="_blank">CTX115712</a>: No User Data for EdgeSight EndPoint Reports

<a href="http://support.citrix.com/article/CTX115855" target="_blank">CTX115855</a>: Citrix Presentation Servers Imaged with EdgeSight Agent are Not Reporting to EdgeSight

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*