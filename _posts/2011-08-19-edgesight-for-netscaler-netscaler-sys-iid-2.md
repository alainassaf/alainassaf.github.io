---
layout: post
title: NetScaler SYS.IID
subtitle: EdgeSight for NetScaler
date: 2011-08-19
tags: [Administration, Citrix, EdgeSight, EdgeSight for NetScaler, Reporting, Monitoring]
---
The new version of EdgeSight for NetScaler 2.1 was released 7/21/2011.  You can get it <a href="https://www.citrix.com/English/ss/downloads/details.asp?downloadId=2302513&amp;productId=25119">here</a> (mycitrix login required).

I'm setting up the new  EdgeSight for NetScaler 2.1 and need to add my NetScalers to it.  Here’s a quick step-by-step on adding a NetScaler.

Log into the Web Console and Click on the Administration tab:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-netscaler-sys-iid-2/image8.png" width="1028" height="224" alt="image8">

Expand Company Settings, then Server, then click on Device Management.  Then click the New Registration button.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-netscaler-sys-iid-2/image9.png" width="1028" height="358" alt="image9">

The fields you need are the NetScaler Name, the NetScaler SYS.IID, the NetScaler IP address, and the fully qualified domain name.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-netscaler-sys-iid-2/image10.png" width="644" height="273" alt="image10">

The NetScaler name is just a identifier for the console.  The NSIP address is just the main IP for the NetScaler.  The FQDN is your full domain.  The SYS.IID is less obvious however.  You will have to access the command-line interface of your NetScaler in order to get this number.  The built-in help will walk you through how to find this information:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-netscaler-sys-iid-2/image11.png" width="644" height="276" alt="image11">

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-for-netscaler-netscaler-sys-iid-2/image12.png" width="644" height="343" alt="image12">

Once you fill in this information, your Data Collector will update and EdgeSight will begin collecting data.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*