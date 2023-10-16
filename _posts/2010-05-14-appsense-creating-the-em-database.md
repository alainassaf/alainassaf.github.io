---
layout: post
title: Creating the EM database
subtitle: AppSense
date: 2010-05-14
tags: [AppSense, Citrix, Environment Manager, MS SQL, SQL, User, Virtualization]
---
We are in the midst of implementing AppSense to manage our Citrix users and eventually all users in the enterprise.  It has been hard to find information in the blogosphere which I find surprising.  

AppSense is not a brand new product and its recent [**recognition**](https://appsense.wordpress.com/2010/05/11/appsense-named-the-citrix-ready-2010-solution-of-the-year-winner/){:target="_blank"} by Citrix at this year’s Synergy leads me to believe that someone (other than AppSense sales) is talking about this product.  I would greatly appreciate if any readers of this blog could suggest any AppSense blogs they’ve come across.

<strong>Scenario</strong>

In most enterprise environments, there is compartmentalization of IT services and being on the Citrix team, I do not have sysadmin access to our enterprise MS SQL cluster.  This was not an issue when I setup the Management database as the service account for that DB only needs DBO privileges.  I spent several days pouring over AppSense documentation, searching Google, and communicating with my DBA and AppSense support to get this database created.  Partially, this was my fault because the documentation I had did not make the following steps clear to myself or my DBA.  I’m including the steps forwarded to me from AppSense support and the PDF they sent that describes this process.
<ol>
	<li>Create 2 accounts (Windows or SQL Accounts) - example AppSenseConfig and AppSenseService</li>
	<li>Ensure both of these accounts do not have an expiry on the password</li>
	<li>On the SQL Server, map the roles of Sysadmin to the AppSenseConfig account</li>
	<li>On the SQL Server, do not map any roles for the AppSenseService account</li>
	<li>Launch the Server Configuration Server and run the wizard using the 2 newly created accounts. At the first screen, enter the details from the AppSenseConfig account and point to the SQL Server and the existing database AppSenseEMDB</li>
	<li>At the next screen, enter the details of the AppSenseService account</li>
	<li>Complete the rest of the wizard.</li>
	<li>On completion, ensure that under the database tree, the AppSenseEMDB database is connected and not highlighted red. If any of the tree view items are marked red, select the Variances button, take a screenshot of the variance(s) and then select repair. If the variances cannot be fixed, contact AppSense support.</li>
	<li>Once the upgrade to the database has completed, you can remove the Sysadmin rights to the AppSenseConfig account as per the details in the PDF. This document explains all the roles during and post install of the accounts used in the setup of the database.</li>
</ol>

**PDF:** [**Installing AppSense Personalization Server with Limited Database Privilege**](/assets/img/appsense-creating-the-em-database/installing-appsense-personalization-server-with-limited-database-privilege.pdf)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*