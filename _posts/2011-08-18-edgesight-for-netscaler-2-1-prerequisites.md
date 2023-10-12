---
layout: post
title: EdgeSight for NetScaler 2.1 Prerequisites
date: 2011-08-18
tags: [Citrix, EdgeSight, EdgeSight for NetScaler, Netscaler, Reporting, Monitoring]
---
I'm finally getting around to installing the new EdgeSight for NetScaler version 2.1.  Here are the prerequisites you will need to get started.

<em>NOTE: I'm installing this on a Windows 2008 64-bit server updated with August 2011 updates</em>

<strong>From the documentation</strong>

<span style="text-decoration:underline;">Hardware</span>
<ul>
	<li>CPU: 2GHz (or better)</li>
	<li>Memory: 2GB of RAM recommended, 1GB of RAM required</li>
	<li>Disk: 2GB free space</li>
</ul>
<span style="text-decoration:underline;">Software</span>
<ul>
	<li>Windows Server 2008 or Windows Server 2003 SP1 or later. Both 32-bit and 64-bit systems are supported on all platforms</li>
	<li>Internet Information Services (IIS) 7.0 for Windows Server 2008</li>
	<li>Microsoft Message Queuing (MSMQ)</li>
	<li>Microsoft Distributed Transaction Coordinator (MSDTC)</li>
	<li>ASP.NET</li>
	<li>Microsoft XML Parser 3.0</li>
	<li>Windows Script 5.6 or higher</li>
	<li>.NET Framework 2.0 SP1</li>
	<li>SQL Client Add-On Tools, including SQL-DMO objects.(if Web server and database server are on different machines)</li>
</ul>

<strong>Reported by the Installer</strong>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-2-1-prerequisites/image-003.png" width="474" height="387" alt="image-003">

<ul>
	<li>SQL Server Client Tools Requirement</li>
<ul>
	<li>Look for SQLServer2005_BC.msi or SQLServer2005_BC_x64.msi on <a href="http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=24793">http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=24793</a></li>
</ul>
	<li>Operating System Requirement</li>
	<li>IIS Feature Requirement</li>
<ul>
	<li>Role Services</li>
<ul>
	<li>Static Content</li>
	<li>Default Document</li>
	<li>ASP.NET</li>
	<li>ISAPI Extensions</li>
	<li>ISAPI Filters</li>
	<li>Windows Authentication</li>
</ul>
	<li>Ensure that the following Management Tools are selected under Role Services for the Web Server:</li>
<ul>
	<li>IIS 6 Management Compatibility</li>
	<li>IIS 6 Metabase Compatibility</li>
	<li>IIS 6 WMI Compatibility</li>
	<li>IIS 6 Scripting Tools</li>
	<li>IIS 6 Management Console</li>
</ul>
</ul>
	<li>Microsoft .NET 2.0 SP1 Runtime Requirement</li>
	<li>MSMQ Requirement</li>
<ul>
	<li>Add Features: Message Queuing -&gt; Message Queuing Services</li>
</ul>
	<li>MSXML Requirement</li>
	<li>Windows Script Host Requirement</li>
</ul>

After adding the above components...

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-2-1-prerequisites/image-0051.png" width="477" height="385" alt="image-0051">

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*