---
layout: post
title: Reporting On Non-PC Devices
subtitle: EdgeSight
date: 2011-07-13
tags: [Citrix, EdgeSight, Endpoints, Mobility, MS SQL, Receiver, Reporting, Monitoring, SQL, XenApp]
---
<strong>UPDATE</strong>: <em>Added new WHERE statement to select just iOS devices (see below).</em>

<strong>Intro</strong>

Today’s workplace no longer follows a strict standard in terms of endpoint devices.  Despite the efforts of your infrastructure, network, and security teams users are connecting non-approved devices to your network and your Citrix farm.  A lot has been said about the “<a href="https://www.webopedia.com/definitions/consumerization-of-it/#:~:text=Consumerization%20of%20IT%20%28%E2%80%9Cconsumerization%E2%80%9D%29%20is%20a%20phrase%20used,home%20and%20then%20introducing%20them%20in%20the%20workplace."><b>Consumerization of IT</b></a>” and it is a reality for any Citrix administrator/engineer.  In this blog post we will explore how to find these types of devices using EdgeSight.

<strong>The Query</strong>

We will use the `VW_ES_USERGROUP_ICA_USERS` view for this query.  Here are the columns in this view:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-non-pc-devices/image.png" width="288" height="484" alt="image">

Here is a sample of data in this view (customer specific information hidden):

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-non-pc-devices/image1.png" width="800" height="129" alt="image1">

<strong>Mobile Devices</strong>

The following query will select mobile devices that connected to your farm in the last 30 days.

```sql
SELECT CONVERT(VARCHAR,dtlast,111) AS 'Date', account_name, client_buildnum, client_productid, client_disp_horiz, client_disp_vert
FROM vw_es_usergroup_ica_users
WHERE client_name = 'mobile'
and account_name &lt;&gt; 'UNKNOWN'
and CONVERT(VARCHAR,dtlast,111) &gt;= getdate() - 30
ORDER BY 'Date' DESC
```

<strong>UPDATE: </strong>While working on a similar query for work, I found that you may also select iOS devices by using the following in your WHERE statement

```sql
WHERE client_name like 'iOS%'
```

Here’s a sample of the output:
<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-non-pc-devices/image2.png" width="644" height="238" alt="image2">

The new Citrix Receiver sets the client name to ‘mobile’. On a PC this is typically the environment variable `%COMPUTERNAME%`.  To find the devices that are connecting, you can use the horizontal (client_disp_horiz) and vertical (client_disp_vert) resolutions and compare them to current resolutions of mobile devices.  I found a nice reference list <a href="http://cache8.groovypost.com/wp-content/uploads/2011/04/image230.png">here</a>.  This can get you half-way there.  The only other way that I’ve been able to distinguish the client that is connecting are the ‘client_buildnum’ and ‘client_productid’ fields.  Unfortunately, finding an updated list of ICA/Receiver build numbers is <a href="http://www.google.com/search?q=ica+client+build+number">not easy</a>.  <a href="http://www.archy.net/about/">Stephane Thirion</a> at <a href="http://www.archy.net/">Archy.net</a> provides a <a href="http://www.archy.net/2009/11/23/citrix-xenapp-ica-online-plug-in-client-version-numbers/">recently updated list</a>.

<strong>Thin Clients</strong>

We can also use `VW_ES_USERGROUP_ICA_USERS` to report on thin client devices.  It is unlikely that thin clients will be an unapproved device on your network, but we can get some useful data on them from this view.  The following query will select thin client devices that connected to your farm in the last 30 days.

```sql
SELECT CONVERT(VARCHAR,dtlast,111) AS 'Date', account_name, client_directory, client_version, client_buildnum, client_productid, client_disp_horiz, client_disp_vert
FROM vw_es_usergroup_ica_users
WHERE client_directory like '\%'
and account_name &lt;&gt; 'UNKNOWN'
and CONVERT(VARCHAR,dtlast,111) &gt;= getdate() - 30
ORDER BY 'Date' DESC
```

Here’s a sample of the output:
<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-non-pc-devices/image3.png" width="644" height="169" alt="image3">

If the thin client is windows-based, chances are the client_version field will give you the currently installed ICA client on the device.  You can use this information to pester the person in charge of thin client’s to update them or replace them (just kidding - but really you need to get them updated).  For this example, we examined the ‘client_directory’ column and determined that if it started with a  ‘\’, it was a thin client.  You may have to experiment with this field depending on which thin clients you have in your environment.

I hope this post has shown you how to track down non-pc devices connecting to your Citrix farm.  Once you have determined the ICA/Java client versions connecting to your farm (see the ICA Client Version report in EdgeSight!) you can modify these queries to find Java client users and Macintosh users.

As always I welcome all comments and questions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*