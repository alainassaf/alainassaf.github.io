---
layout: post
title: Timezone offsets
subtitle: EdgeSight
date: 2012-05-07
tags: [EdgeSight, MS SQL, Reporting, Monitoring, SQL]
---
<strong>Intro</strong>

If you have implemented any of the ad hoc SQL queries available on this site, you may have noticed that most time queries are offset by –4 or –5 hours. This is because the EdgeSight database uses <a href="http://en.wikipedia.org/wiki/Greenwich_Mean_Time" target="_blank">GMT</a> to record time and I am located in the U.S. Eastern Time Zone.

In this post we will take a look at some tables in the EdgeSight database that you can use to make your queries more local and portable.

<strong><a href="/assets/img/edgesight-timezone-offsets/seamonster.jpg"><img style="background-image:none;margin:5px 0 5px 5px;padding-left:0;padding-right:0;display:inline;float:right;padding-top:0;border-width:0;" title="seamonster" src="/assets/img/edgesight-timezone-offsets/seamonster.jpg" alt="seamonster" width="244" height="172" align="right" border="0" /></a>There Be Monsters Here!</strong>
<p align="left">Most of my experience with EdgeSight has been with the database views that summarize and organize the vast amount of data that EdgeSight collects. On occasion I’ve gone where few dare to tread to look directly at the tables for the data I need.</p>
<p align="left">EdgeSight’s views are dizzying enough, but the table structure of the EdgeSight database is intimidating to the SQL neophyte. Despite this, I decided to look deeper after David did his post on <a href="https://edgesightunderthehood.wordpress.com/2011/08/17/average-session-count-by-day-and-hour-the-query/" target="_blank">session counts</a>. His query uses the ‘timezone’ table to find the time offset for his query and this got me curious. How can I use this to make my queries easier to maintain and more portable?</p>
<p align="left"><strong>Timezone table</strong></p>
<p align="left">Lets take a look at the timezone table</p>

```sql
SELECT *
FROM timezone
```

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-timezone-offsets/image23.png" alt="image23">
<p align="left">The above picture is only part of the table. It consists of 74 rows. Yeah makes total sense right? Naturally, I had to do some more checking. If you check the company table, we get a clue.</p>

```sql
SELECT *
FROM company
```

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-timezone-offsets/image24.png" alt="image24">

As you can see in the above picture, each company in the EdgeSight database has an associated Time Zone and Language. In this case, we have a timezone id (tzid) of 13 and a culture_name of en-US. If we cross reference the tzid with the timezone table we get:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-timezone-offsets/image25.png" width="644" height="58" alt="image25">

Looking at the result above, we can see that this is for the U.S. Eastern time zone and includes daylight savings time as well. You can configure this in the EdgeSight console by clicking on the Configure tab. Look under the Server Configuration section and click on Companies to see where to add/edit company information.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-timezone-offsets/image26.png" alt="image26">

<p align="left">So for the example above, I have the language set to English and the time zone set to U.S. Eastern Time which has a GMT offset of –5 hours.</p>
<p align="left"><strong>How does this help me?</strong></p>
<p align="left">Let’s take a look at a <a href="http://edgesightunderthehood.com/2011/08/10/finding-users-on-your-network-using-pcs-and-running-a-certain-application/" target="_blank">query</a> I’ve posted on this site before:</p>

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

<p align="left">As you can see above, all the timedate fields are offset by –4 hours. To keep from having to change the offset to –5 or –4 depending on what time of year it was (standard vs. daylight savings time), I developed a simple select query that determines the current offset by checking the timezone table.</p>

```sql
DECLARE @tzbias INT
SELECT @tzbias = case when use_daylight = '0' then standard_bias else daylight_bias end from timezone where tzid = 13
```

In layman’s terms, look at the timezone table where the timezone id (tzid) is equal to 13. If the field 'use_daylight' is equal to zero, use the 'standard_bias' otherwise use the 'daylight_bias'.

I’m setting whatever this query returns equal to the variable @tzbias. I then use the @tzbias variable in my timedate fields in my queries. If we rewrite the above query with the tzbias variable, we get the following:

```sql
DECLARE @tzbias INT
SELECT @tzbias = case when use_daylight = '0' then standard_bias else daylight_bias end from timezone where tzid = 13
DECLARE @today datetime
DECLARE @app varchar(20)
SET @today = convert(varchar,getdate(),111)
SET @app = 'notepad.exe'
SELECT DISTINCT CONVERT(VARCHAR(10),DATEADD(mi,@tzbias,apptbl.time_stamp), 111) AS 'Date', serv.machine_name AS 'Server', serv.[user] AS 'Username', serv.client_name, serv.client_address, serv.client_version, icatbl.client_directory, apptbl.app_description, apptbl.exe_name, apptbl.exe_version
FROM vw_es_archive_application_usage apptbl, vw_ctrx_archive_server_start_perf serv, vw_es_usergroup_ica_users icatbl
WHERE apptbl.exe_name like '%'+@app+'%'
and apptbl.account_name &lt;&gt; 'UNKNOWN'
and serv.client_address not like '192%'
and icatbl.client_directory not like '\%'
and convert(varchar(10),dateadd(mi,@tzbias,apptbl.time_stamp), 111) &gt;= @today-30
and apptbl.sessid = serv.sessid and icatbl.sessid = serv.sessid
and CONVERT(VARCHAR(10),DATEADD(mi,@tzbias,apptbl.time_stamp), 111) = CONVERT(VARCHAR(10),DATEADD(mi,@tzbias,serv.time_stamp), 111)
ORDER BY CONVERT(VARCHAR(10),DATEADD(mi,@tzbias,apptbl.time_stamp), 111), 'username'
```

Since the timezone bias is in minutes, I had to change the DATEADD functions to use mi for minutes. Now I can use my queries year around without worrying about daylight savings time changes.

I hope this provides you some options when doing ad hoc queries against the EdgeSight database. As always, I welcome all comments and questions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*