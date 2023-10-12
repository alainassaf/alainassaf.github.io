---
layout: post
title: Five (very cool) things you didn't know were possible with NetScaler
subtitle: Citrix Synergy 2011
date: 2011-05-31
tags: [Business Intelligence, Citrix, Citrix Synergy, Netscaler, Reporting, Monitoring, SQL]
---
SYN211: Wednesday May 25th 2011

Presented by Ajay Soni, <a href="http://community.citrix.com/pages/viewpage.action?pageId=131334172" target="_blank">Anil Shetty</a>, <a href="http://community.citrix.com/pages/viewpage.action?pageId=84148655" target="_blank">Greg Smith</a>, <a href="http://community.citrix.com/citrite/rajivm/profile" target="_blank">Rajiv Mirani</a>, and Vijay Ratnam

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/20110525-023112.jpg" width="579" height="432" alt="20110525-023112.jpg">

Citrix's acquisition of NetScaler was just the beginning.  With Mark Templeton’s keynote the reach of the NetScaler brand has lengthened to include NetScaler Cloud Gateway and NetScaler Could Bridge.  Meanwhile the NetScaler engineering team has not been idle.  I will briefly cover the talking points from this session.  The following features are/will be available on n-core NetScalers running firmware 9.3 (or later).
<ol>
	<li>SR-IOV</li>
	<li>Network Analytics</li>
	<li>NetScaler Director</li>
	<li>NetScaler Pools</li>
	<li>DataStream</li>
</ol>

<h3>SR-IOV - Single Route IO Virtualization</h3>
Customers have asked that the NetScaler be a multi-tenant device with the ability host multiple applications with CPU and memory isolation.  So the NetScaler and XenServer engineers threw their 2 products into a locked room with some romantic music and let nature take its course.  The result is <a href="http://www.citrix.com/English/ps2/products/subfeature.asp?contentID=2310252" target="_blank">NetScaler SDX</a>.  This is a hardware appliance running a number of NetScaler VPX’s on top of the XenServer hypervisor.  The two teams discovered that when you virtualize the NetScaler to provide this functionality, you can reduce performance. In order to solve this issue the NetScaler team has implemented SR-IOV (Single Route IO Virtualization).

This industry-standard technology allows the virtualization of the NIC into multiple-virtual instances and provides them to NetScaler VPX instances.  They are able to bypass the hypervisor and avoid the reduction in performance as seen below

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image.png" width="580" height="276" alt="image">

Each instance gets full network isolation for layer 3 and above and layer 2 isolation via VLAN tagging (of each instance on the NetScaler SDX).

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image1.png" width="580" height="340" alt="image1">

In order to configure all this, the NetScaler team has provided a Service VM.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image2.png" width="580" height="254" alt="image2">

All the typical VM management operations are available except snapshots.  This is coming as soon as the NetScaler team determines how to snapshot an instance and not have an IP conflict.  Also, the instances are discreet within their SDX appliance.  You cannot failover a VPX instance from one SDX machine to another, but later I will cover NetScaler pools which can solve this issue.
<h3>Network Analytics</h3>
The next feature that is in development is an analytics engine.  This will allow for better troubleshooting, capacity planning, quantifying of the user experience, application monitoring and business intelligence, basically providing the features that EdgeSight provides in XenApp.

The following features not only establish the NetScaler as superior Application Delivery Controller but are also the building blocks for an analytic device.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image3.png" width="580" height="317" alt="image3">

During the presentation, several use cases were presented.  One showed the impact of a traffic spike in the analytic engine and its resolution and the other showed how to create rules to auto cache “busy” objects.  The following screenshot lists the expressions that are used to create these rules (image inverted for visibility):

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image4.png" width="580" height="313" alt="image4">

Other use cases included:

Application capacity planning

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image5.png" width="580" height="395" alt="image5">

Application Monitoring

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image6.png" width="580" height="329" alt="image6">

and Business Intelligence

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image7.png" width="580" height="329" alt="image7">

<h3>NetScaler Director</h3>
NetScaler supports several taps into the data it collects as demonstrated by the following graphic:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image8.png" width="580" height="323" alt="image8">

Unfortunately, the raw data collected is hard to interpret:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image9.png" width="580" height="237" alt="image9">

For this use case, they combined Syslog data with Google Maps to visualize users connecting to the resources on a world map.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image10.png" width="580" height="374" alt="image10">

This example was very demonstrative of the data that NetScaler can collect and merge with other data sets.  However, there was scant detail on how NetScaler Director will look in the future or what functionality it will have.

<h3>NetScaler Pools</h3>
The next development is the concept of a NetScaler Pool.  This will allow you to create pools of NetScalers (VPX) that will dynamically failover and recover.  Typically most deployments will have either an active-passive or active-active HA NetScaler pair.

Using VRRP (Virtual Redundant Routing Protocol), VIP’s are given different priorities depending on different NetScalers.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image11.png" width="580" height="313" alt="image11">

Expanding on this concept we can create an N+1 HA configuration.  Essentially this leaves one NetScaler that contains all the VIPs in a lower priority as a hot spare.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image12.png" width="580" height="346" alt="image12">

NetScalers support automatic failover and recovery and when used with a NetScaler SDX, it creates a very robust HA environment, but there are some issues to note:
<ul>
	<li>No Automatic NetScaler Configuration Synchronization</li>
	<li>Session Consistency is supported only in some protocols (these were not listed)</li>
	<li>Each NetScaler has to independently monitor its own services</li>
	<li>Requires NetScaler nCore build</li>
</ul>

<h3>DataStream</h3>
Frankly, this feature is the most significant one presented and it is now available.  DataStream allows you to leverage the power of the NetScaler to accelerate your database tier.

For most organizations databases have the following challenges:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image13.png" width="580" height="315" alt="image13">

By utilizing DataStream in your Database Tier, you gain the following benefits:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image14.png" width="580" height="280" alt="image14">

NetScaler has been a proven platform to accelerate web sites, but leveraging it to accelerate database transactions is huge.  Inserting the NetScaler in front of your Web/App servers allows you to support millions of client-side connections, faster connection establishment to the backend databases, SQL proxy support for MySQL and MSSQL, and granular SQL policy control.  At the same time the NetScaler provides fewer multiplexed server-side connections to the database servers, longer lived server-side connections, and native SQL request/response protocol visibility.

The demo of this technology is summarized in the following screenshots:

First the demo query without the NetScaler.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image15.png" width="537" height="484" alt="image15">

Then a SQL Virtual Server was created on the NetScaler.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/citrix-synergy-five-very-cool-things-you-didnt-know-were-possible-with-netscaler/image16.png" width="595" height="484" alt="image16">

Keep in mind that in this demo the same query was used over and over, but this shows how the caching and acceleration in the NetScaler can improve transactions with the database tier.  Also note that a single MPX was used  and you can imagine if this technology is implemented on the NetScaler SDX.

<h3>Summary</h3>
There were several other sessions that covered various NetScaler technologies.  I urge you to view the Synergy sessions that are now available on the Citrix site.

+ SYN211: Five (very cool) things you didn’t know were possible with NetScaler
+ Scaling the Data Tier with Citrix NetScaler DataStream Technology
+ Synergy Live!

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*