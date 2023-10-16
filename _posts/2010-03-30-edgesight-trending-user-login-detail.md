---
layout: post
title: Trending User Login Detail
subtitle: With EdgeSight
date: 2010-03-30
tags: [Citrix, EdgeSight, MS SQL, Repost, Reporting, Monitoring, SQL, XenApp]
---
My colleague John Smith has started a second blog at [**EdgeSight Under the Hood**](https://edgesightunderthehood.wordpress.com/){:target="_blank"}.  He delves into the vast amount of data that EdgeSight collects and uses SQL queries to present this information in meaningful ways. Inspired by him, I'm going to write about averaging user login metrics that EdgeSight collects, but only reports them on a per user basis.

<strong>EdgeSight Views</strong>

The EdgeSight database schema is nightmarish, but Citrix collects most of the data in database views (which are basically permanent queries).  The particular view we're going to look at is the vw_ctrx_archive_server_start_perf view.  Here's a breakdown of the columns and what they represent:

<strong>View Table: vw_ctrx_archive_server_start_perf</strong>
<table border="0">
<tbody>
<tr>
<th>Startup detail</th>
<th>Server Session Startup Column</th>
</tr>
<tr>
<td>Credentials Authentication</td>
<td>credentials_authentication_server_duration</td>
</tr>
<tr>
<td>Credentials Obtention</td>
<td>credentials_obtention_server_duration</td>
</tr>
<tr>
<td>Device Mapping</td>
<td>device_mapping_server_duration</td>
</tr>
<tr>
<td>Login Script Execution</td>
<td>login_script_execution_server_duration</td>
</tr>
<tr>
<td>Profile Load</td>
<td>profile_load_server_duration</td>
</tr>
<tr>
<td>Session Creation</td>
<td>session_creation_server_duration</td>
</tr>
<tr>
<td>Printer creation</td>
<td>printer_creation_server_duration</td>
</tr>
<tr>
<td>Session Startup Duration</td>
<td>Session_startup_server</td>
</tr>
</tbody>
</table>
<strong> </strong>

<strong>Scenario</strong>

Users are reporting that it's taking longer to log into your Citrix farm than a week or two ago.  Suspecting that your AD team has been messing around with the domain login script again you want to see if there's been a measurable change in how long it takes to process the login script.

Here's the query:

```sql
declare @today datetime
set @today = convert(varchar,getdate(),111)
select convert(varchar(10),dateadd(hh,-4,time_stamp), 111) as [Date], convert(decimal(19,2),avg(login_script_execution_server_duration)/1000.0) as 'Login Script (sec)',  convert(decimal(19,2),avg(Session_startup_server)/1000.0) as 'Session StartUp Total (sec)'
from vw_ctrx_archive_server_start_perf
where convert(varchar(10),dateadd(hh,-4,time_stamp), 111) &gt; @today-30
group by convert(varchar(10),dateadd(hh,-4,time_stamp), 111)
order by convert(varchar(10),dateadd(hh,-4,time_stamp), 111)
```

This takes the average for all logins (captured by EdgeSight) and averages the login script processing time and the total session startup time.
<h6>NOTE: In the dateadd functions you'll notice a (-4).  This is necessary to offset the data stored in EdgeSight with your time zone.  EdgeSight stores its data based on UTC time.  Using the -4 will move it to Eastern (U.S.) daylight savings time (normally it's UTC -5).  Keep this in mind if you do ad hoc queries with EdgeSight data.</h6>
EdgeSight will, by default, only store this type of information for 30 days.  If you want to report on longer periods, you'll have to save the information in another form or change the EdgeSight worker to not purge data as often.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*