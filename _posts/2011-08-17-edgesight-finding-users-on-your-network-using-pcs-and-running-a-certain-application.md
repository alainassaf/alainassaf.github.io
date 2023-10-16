---
layout: post
title: Finding Users On Your Network, Using PC's, and Running a Certain Application
subtitle: EdgeSight
date: 2011-08-17
tags: [EdgeSight, MS SQL, Reporting, Monitoring]
---
<strong>Intro</strong>

Recently I was asked to determine which users were using a certain application in our Citrix Farm.  We are using a published desktop and while EdgeSight has reports to show published applications, few built-in reports to show what users are running in their session.  In addition, I was only looking for users who were on our internal network and not using a thin client.  Unless your network team has created a very segregated network, and you have setup user groups based on various subnets and devices, this sort of information is impossible to pull out of EdgeSight.   In this post I will show you a query that gathers this information.

<strong>The Query</strong>

```sql
DECLARE @today datetime
DECLARE @app varchar(20)
SET @today = convert(varchar,getdate(),111)
SET @app = 'notepad.exe'
SELECT DISTINCT CONVERT(VARCHAR(10),DATEADD(hh,-4,apptbl.time_stamp), 111) AS 'Date', serv.machine_name AS 'Server', serv.[user] AS 'Username', serv.client_name, serv.client_address, serv.client_version, icatbl.client_directory, apptbl.app_description, apptbl.exe_name, apptbl.exe_version
FROM vw_es_archive_application_usage apptbl, vw_ctrx_archive_server_start_perf serv, vw_es_usergroup_ica_users icatbl
WHERE apptbl.exe_name like '%'+@app+'%'
and apptbl.account_name &lt;&gt; 'UNKNOWN'
and serv.client_address not like '192%'
and icatbl.client_directory not like '\%'
and convert(varchar(10),dateadd(hh,-4,apptbl.time_stamp), 111) &gt;= @today-30
and apptbl.sessid = serv.sessid and icatbl.sessid = serv.sessid
and CONVERT(VARCHAR(10),DATEADD(hh,-4,apptbl.time_stamp), 111) = CONVERT(VARCHAR(10),DATEADD(hh,-4,serv.time_stamp), 111)
ORDER BY CONVERT(VARCHAR(10),DATEADD(hh,-4,apptbl.time_stamp), 111), 'username'
```

<strong>The Query Explained</strong>

Let’s review the criteria we are looking for in this query:
<ol>
	<li>Users accessing a certain application</li>
	<li>Users who are not using thin clients</li>
	<li>Users who are on the internal LAN</li>
</ol>
To gather this information, I’m using 3 different views in the EdgeSight database:
<ol>
	<li>vw_es_archive_application_usage - aliased as “apptbl”
<ul>
	<li>This will give me Application Description, the EXE name, the EXE version</li>
</ul>
</li>
	<li>vw_ctrx_archive_server_start_perf - aliased as “serv”
<ul>
	<li>This will give me the XenApp server, the Username, the Client Name, IP Address, and ICA Version</li>
</ul>
</li>
	<li>vw_es_usergroup_ica_users - aliased as “icatbl”
<ul>
	<li>This will give me the ICA Client Directory</li>
</ul>
</li>
</ol>
These 3 views will be linked by the SESSID (session id) column with is present in all the views.

```sql
and apptbl.sessid = serv.sessid and icatbl.sessid = serv.sessid
```

First we declare some variables and assign them values:

```sql
DECLARE @today datetime
DECLARE @app varchar(20)
SET @today = convert(varchar,getdate(),111)
SET @app = 'notepad.exe'
```

If you have the several requests with different criteria you can declare some variables to help you.  In this case, I’ve created a variable called @app that I can set to any executable that I’m reporting on. To refer to this variable in the query, I use it in the WHERE clause using a LIKE operator and a regular expression.

```sql
WHERE apptbl.exe_name like '%'+@app+'%'
```

The rest of the WHERE clause helps us find the users we are looking for.

```sql
and apptbl.account_name &lt;&gt; 'UNKNOWN'
and serv.client_address not like '192%'
and icatbl.client_directory not like '\%'
and convert(varchar(10),dateadd(hh,-4,apptbl.time_stamp), 111) &gt;= @today-30
and apptbl.sessid = serv.sessid and icatbl.sessid = serv.sessid
and CONVERT(VARCHAR(10),DATEADD(hh,-4,apptbl.time_stamp), 111) = CONVERT(VARCHAR(10),DATEADD(hh,-4,serv.time_stamp), 111)
```

I have filtered out user IP addresses that start with “192” as this is typical of home-based routers.  Obviously, you can modify this to reflect your own network.  To filter out thin-clients, I’m not selecting any client directories that start with “\”.  I’ve found that thin clients (in my case Wyse) have file systems that begin with a “\” and you can refer to my post that covered finding non-PC devices in EdgeSight <a title="EdgeSight: Reporting On Non-PC Devices" href="/2011-07-13-edgesight-reporting-on-non-pc-devices/"><b>here</b></a>. Finally, I’m only looking at entries for the past 30 days, where the sessid’s match, and where the time_stamps match.

I always welcome comments and questions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*