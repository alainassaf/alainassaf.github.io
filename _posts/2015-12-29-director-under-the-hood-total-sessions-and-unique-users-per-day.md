---
layout: post
title: Total Sessions and Unique Users Per Day
subtitle: Director Under the Hood
date: 2015-12-29
readtime: true
tags: [MS SQL, Reporting, Reporting, Monitoring, SQL]
---
# Intro #
Director is Citrix's new metrics and monitoring dashboard. The interface is modern and the emphasis is on real-time information about your users. It consolidates information about your environment and makes it easy to differentiate between applications and desktops. If your only experience has been with EdgeSight in the past then you'll see Director as a breath of fresh air.

There's a lot of good views and data in the new Citrix Director and the "one pane of glass" view of your environment is pursued by all 3rd party monitoring, reporting, and alerting vendors. Unfortunately, it's not easy to get all the same data I've gathered in past from the Director database. In this post we'll look at a query to show you the total sessions and unique users per day.

## This is it…really? ##

| The tables that make up the Director Database | The views that make up the Director Database |
| :---: | :---: |
| ![image](/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image.png) | ![image1](/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image1.png) | 


After years of pouring through and querying EdgeSight’s tables and views, I first thought that something must be wrong. This can’t be all there is to the Director database, but that’s all there is.  Before we dive into SQL, let’s see what we can find using the Director GUI. I like to collect lots of metrics when I report on my environment. The 3 main session metrics I track are concurrent user per day, unique users per day and total sessions per day. Can we find this info in the Director Trends dashboard?

I set the Time period to Last Month and then set to custom ending to 10/1/2015. This should give me data for September 2015. Here’s what we get:

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image2.png" 
    alt="image2">

<h6><span style="font-weight:bold;">NOTE: For these examples, I’m looking at all delivery groups. You can limit your view by delivery group if you wanted to track metrics for different groups of users.</span></h6>

As you can see, we get a pretty graph, but we have to export the data to Excel to get precise detail:

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image3.png" 
    alt="image3">

What this doesn’t show us it how many sessions and unique users there are per day. The only way to get this using the Director interface is to click on a point on the graph to see the session details. This will only work for more recent time period.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image4.png" 
    alt="image4">

# SQL To the Rescue #

For this query I’m using the following tables/views:

| MonitorData.SessionV1 (View) | MonitorData.Connection (Table) | MonitorData.User (Table) |
| :---: | :---: | :---: |
| ![image5](/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image5.png) | ![image6](/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image6.png) | ![image7](/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image7.png) |

I’m linking the SessionV1 and Connection SessionKey columns together and the **User.id** and **SessionV1.userid** columns together. This ensures that I’m grouping the same sessions and users together (users can have more than one session). 
Then I group by the **LogOnStartDate** and count the distinct sessionkeys and distinct userids. This gives me the total sessions and unique users per day.

This query will pull all available data and total the sessions and unique users per day.
```sql
select convert(varchar(10),LogOnStartDate,111) as 'Date', count (distinct MonitorData.SessionV1.sessionKey) as 'Total Sessions', count (distinct MonitorData.SessionV1.Userid) as 'Unique Users'
from MonitorData.SessionV1,MonitorData.Connection,MonitorData.[User]
where FailureDate is NULL and MonitorData.SessionV1.SessionKey = MonitorData.Connection.SessionKey
and MonitorData.[User].Id = MonitorData.SessionV1.userid
group by convert(varchar(10),LogOnStartDate,111)
order by convert(varchar(10),LogOnStartDate,111)
```

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image8.png" 
    alt="image8">

The following query is similar, but it just pulls data for the current month.

```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
select convert(varchar(10),LogOnStartDate,111) as 'Date', count (distinct MonitorData.SessionV1.sessionKey) as 'Total Sessions', count (distinct MonitorData.SessionV1.Userid) as 'Unique Users'
from MonitorData.SessionV1,MonitorData.Connection,MonitorData.[User]
where FailureDate is NULL and MonitorData.SessionV1.SessionKey = MonitorData.Connection.SessionKey
and MonitorData.[User].Id = MonitorData.SessionV1.userid
and convert(varchar(10),LogOnStartDate,111) between CONVERT(VARCHAR(25),DATEADD(dd,-(DAY(@mydate)-1),@mydate),111)
and CONVERT(VARCHAR(25),DATEADD(dd,-(DAY(DATEADD(mm,1,@mydate))),DATEADD(mm,1,@mydate)),111)
group by convert(varchar(10),LogOnStartDate,111)
order by convert(varchar(10),LogOnStartDate,111)
```

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image9.png" 
    alt="image9">

This query groups by the current month, so you can get the total unique sessions and users for the current month:
```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
select convert(char(9),datename(month,LogOnStartDate)) + ' ' + convert(char(4),datepart(year,LogonStartDate)) as 'Month',
count (distinct MonitorData.SessionV1.sessionKey) as 'Total Sessions',
count (distinct MonitorData.SessionV1.Userid) as 'Unique Users'
from MonitorData.SessionV1,MonitorData.Connection,MonitorData.[User]
where FailureDate is NULL
and MonitorData.SessionV1.SessionKey = MonitorData.Connection.SessionKey
and MonitorData.[User].Id = MonitorData.SessionV1.userid
and convert(varchar(25),LogOnStartDate,107) between CONVERT(VARCHAR(25),DATEADD(dd,-(DAY(@mydate)-1),@mydate),107)
and CONVERT(VARCHAR(25),DATEADD(dd,-(DAY(DATEADD(mm,1,@mydate))),DATEADD(mm,1,@mydate)),107)
group by convert(char(9),datename(month,LogOnStartDate)) + ' ' + convert(char(4),datepart(year,LogonStartDate))
```
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image10.png" 
    alt="image10">

This query is similar to above, but takes all the available data and groups it by month:
```sql
select convert(char(9),datename(month,LogOnStartDate)) + ' ' + convert(char(4),datepart(year,LogonStartDate)) as 'Month',
count (distinct MonitorData.SessionV1.sessionKey) as 'Total Sessions',
count (distinct MonitorData.SessionV1.Userid) as 'Unique Users'
from MonitorData.SessionV1,MonitorData.Connection,MonitorData.[User]
where FailureDate is NULL
and MonitorData.SessionV1.SessionKey = MonitorData.Connection.SessionKey
and MonitorData.[User].Id = MonitorData.SessionV1.userid
group by convert(char(9),datename(month,LogOnStartDate)) + ' ' + convert(char(4),datepart(year,LogonStartDate))
```
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-total-sessions-and-unique-users-per-day/image11.png" 
    alt="image11">

# In conclusion #

I hope this encourages you to take a look under the hood of Director to see what you can get out of it. The database infrastructure is much, much simpler than EdgeSight and should provide a lot of good detail.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*