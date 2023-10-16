---
layout: post
title: Filtering a query by IP Subnet
subtitle: EdgeSight
date: 2011-04-28
tags: [Citrix, EdgeSight, MS SQL, Reporting, Monitoring]
---
<img style="background-image:none;padding-left:0;padding-right:0;display:inline;float:left;padding-top:0;border:0;margin:0 15px 0 5px;" title="index" src="/assets/img/icons/es-logo.jpg" alt="" width="134" height="134" align="left" border="0" />

### Prologue
Many of EdgeSight’s tables and views have a field for the client’s IP address, and this is stored as variable-length character string (varchar or nvarchar). In order to sort or filter on this field you must use a complex regular expression or find a way to split the field into different octets. In this blog post, we will do just that by presenting a problem that requires finding users based on their subnet…

# Intro
Thanks to the vibrant competition present in the virtualization space, many Engineers find themselves always transitioning to the next version of their virtualization solution. During such a transition, management (and hopefully the engineers) want to know who’s using the new system and if users are still accessing the old one. In many cases this can be a trivial exercise, but for this scenario we’ll make it more complex.

# Scenario
The networking team has intelligently organized its user’s locations by subnet. In fact, due to number of users and available IP’s, each floor at the main location has it’s own subnet. Recently, Citrix users at the main location were transitioned to the new environment, except for a subset who had legacy applications that would not work in the new Citrix farm. Management wants to know many of the transitioned users are using the new system.

# Problem
Since we are using published desktops in both the old and new Citrix environments, EdgeSight (version 5.3) does not provide an easy way to query desktop launches (see <a href="http://edgesightunderthehood.com/2010/08/25/oye-vey-published-desktops-in-edgesight-5-3/" target="_blank">this post</a> on <a href="http://edgesightunderthehood.com/" target="_blank">EdgeSight Under the Hood</a> for how to get a query of published desktop launches). In this case, we have a different naming schema for the servers in the new farm, but since there are many different locations connecting back to our Citrix farms, we need to just select the users at the main location. This will require us to filter the users based on their IP subnet.

# PARSENAME
While researching this issue I found that dealing with IP addresses in <a href="https://secure.wikimedia.org/wikipedia/en/wiki/Transact-SQL" target="_blank">Transact-SQL</a> is a common problem. Luckily there is a built-in function called <a href="http://msdn.microsoft.com/en-us/library/ms188006.aspx" target="_blank">PARSENAME</a> that parses object names like ‘<em>servername.databasename.schemaname.objectname’. </em>Since IPv4 addresses follow the same convention, you can reference each part of the octet in an IP address.

For example:

```sql
DECLARE @IP nvarchar(15)
SET @IP = '192.168.1.1'
SELECT PARSENAME(@IP,4) AS 'Octet 1',
PARSENAME(@IP,3)AS 'Octet 2',
PARSENAME(@IP,2)AS 'Octet 3',
PARSENAME(@IP,1)AS 'Octet 4'
```

Gives us:

```
Octet 1    Octet 2    Octet 3    Octet 4
---------- ---------- ---------- ----------
192        168        1          1

(1 row(s) affected)
```

# The query
For this query we will use `vw_ctrx_archive_server_start_perf` which has become my goto view for client related information and just sort by one subnet: 192.168.1.0 – 192.168.1.101 and look at the last 3 days of data

```sql
DECLARE @today datetime
SET @today = CONVERT(varchar(10),getdate(),111)
--we are using DATEADD and offsetting by minus four hours due to Eastern Daylight Time
SELECT CONVERT(varchar(10),DATEADD(hh,-4,time_stamp), 111) as 'Date', ([user]) as 'User'
FROM vw_ctrx_archive_server_start_perf
WHERE CONVERT(varchar(10),DATEADD(hh,-4,time_stamp), 111) &gt;= @today-3 --past 3 days
and [user] &lt;&gt; 'UNKNOWN'
--Gets NEWSERVER01, NEWSERVER02, etc
and machine_name like 'NEWSERVER%
and PARSENAME(client_address,4) = '192'
and PARSENAME(client_address,3) = '168'  and (PARSENAME(client_address,2) = 1 and PARSENAME(client_address,1) between 0 and 101
GROUP BY CONVERT(varchar(10),DATEADD(hh,-4,time_stamp), 111), [user]
ORDER BY CONVERT(varchar(10),DATEADD(hh,-4,time_stamp), 111)
```

For our second example, we’ll sort with 11 sub-nets:
```
192.168.1.0 – 192.168.1.101
192.168.2.0 – 192.168.2.102
192.168.3.0 – 192.168.3.103
192.168.4.0 – 192.168.4.104
192.168.5.0 – 192.168.5.105
192.168.6.0 – 192.168.6.106
192.168.7.0 – 192.168.7.107
192.168.8.0 – 192.168.8.108
192.168.9.0 – 192.168.9.109
192.168.10.0 – 192.168.10.110
192.168.11.0 – 192.168.11.121
```

```sql
DECLARE @today datetime
SET @today = convert(varchar(10),getdate(),111)
SELECT CONVERT(varchar(10),dateadd(hh,-4,time_stamp), 111) as 'Date', ([user]) as 'User'
FROM vw_ctrx_archive_server_start_perf
WHERE CONVERT(varchar(10),dateadd(hh,-4,time_stamp), 111) &gt;= @today-3
and [user] &lt;&gt; 'UNKNOWN'
and machine_name like 'NEWSERVER%'
and PARSENAME(client_address,4) = '192'
and PARSENAME(client_address,3) = '168'
and (PARSENAME(client_address,2) =1 and PARSENAME(client_address,1) between 0 and 101
or PARSENAME(client_address,2) =2 and PARSENAME(client_address,1) between 0 and 102
or PARSENAME(client_address,2) =3 and PARSENAME(client_address,1) between 0 and 103
or PARSENAME(client_address,2) =4 and PARSENAME(client_address,1) between 0 and 104
or PARSENAME(client_address,2) =5 and PARSENAME(client_address,1) between 0 and 105
or PARSENAME(client_address,2) =6 and PARSENAME(client_address,1) between 0 and 106
or PARSENAME(client_address,2) =7 and PARSENAME(client_address,1) between 0 and 107
or PARSENAME(client_address,2) =8 and PARSENAME(client_address,1) between 0 and 108
or PARSENAME(client_address,2) =9 and PARSENAME(client_address,1) between 0 and 109
or PARSENAME(client_address,2) =10 and PARSENAME(client_address,1) between 0 and 110
or PARSENAME(client_address,2) =11 and PARSENAME(client_address,1) between 0 and 121)
GROUP BY convert(varchar(10),dateadd(hh,-4,time_stamp), 111), [user]
ORDER BY convert(varchar(10),dateadd(hh,-4,time_stamp), 111)
```

Hopefully this will provide you with some more options when you need to present data from your EdgeSight database. As always I welcome any and all questions and comments.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*