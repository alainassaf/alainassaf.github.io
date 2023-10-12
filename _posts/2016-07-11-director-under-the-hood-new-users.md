---
layout: post
title: New Users
subtitle: Director Under the Hood
date: 2016-07-11
readtime: true
tags: [Citrix Director, MS SQL, Reporting, Monitoring, SQL]
cover-img: [/assets/img/director-under-the-hood-new-users/newusers.jpg]
---
<!--more-->

# Contents

* TOC
{:toc}

# Intro #
Director is Citrix's new metrics and monitoring dashboard. The interface is modern and the emphasis is on real-time information about your users. It consolidates information about your environment and makes it easy to differentiate between applications and desktops. If your only experience has been with EdgeSight in the past then you'll see Director as a breath of fresh air.

There's a lot of good views and data in the new Citrix Director and the "one pane of glass" view of your environment is pursued by all 3rd party monitoring, reporting, and alerting vendors. Unfortunately, it's not easy to get all the same data I've gathered in past from the Director database. In this post we'll look at tracking new users connecting to your Citrix environment.

[For information on the database schema...read my previous article on Director.](/2015-12-29-director-under-the-hood-total-sessions-and-unique-users-per-day/)

# New Users #
I collect lots of metrics to report on my environment. One of the ones I track is the number of new users that connect to my Citrix environment. I view this metric as speaking to the overall adoption rate of my Citrix platform as well as a leading indicator for growth. Can we find this info in the Director Trends dashboard?

The short answer is no. The long answer is noooooooooooooooooooooooooooooo. In fact, it is not possbile to track this in EdgeSight. In a previous job, we worked around this by adding a USER table to the Edgesight database and then ran a query to compare the unique users who logged in that past month against the USER table. Who ever did not show up in the USER table was considered new.

```sql
SELECT distinct [user]
FROM vw_ctrx_archive_server_start_perf AS ESdata
WHERE [user]  'UNKNOWN'
and convert(varchar(10),time_stamp,111) between '2016/05/01'
and '2016/05/31'
and (NOT EXISTS
(SELECT distinct userid
FROM userarchive
WHERE (userarchive.userid = ESData.[user]))) order by [user]
```

The above query gets all the unqiue users who logged in between May 1st and May 31st (using the **Edgesight view: vw_ctrx_archive_server_start_perf)**. It then compares this list against the *userarchive* table that we created to store the username and some other data about our users. Thus we got  a count of new users to our Citrix environment. Once we completed our monthly reporting, we added these new users to the *userarchive* table.

You say, "That's great Alain. Wow! How the heck do I do this in Director?"

I say...
# "SQL To the Rescue!" #
For this query I’m using only one table:

| MonitorData.User (Table) |
| :---: |
| ![monitorData.User](/assets/img/director-under-the-hood-new-users/image7.png) |

I select the month and year and then count the usernames for that month and year. The great thing about this table is that it only creates a new row the first time a user connects to the system automatically. So, the following query will give you a easy way to see the new users who connected to your Citrix envrionment.

```sql
SELECT convert(char(9),datename(month,CreatedDate)) + ' '
+ convert(char(4),datepart(year,CreatedDate)) as 'Month',
count (Username) as 'New Users'
FROM MonitorData.[User]
GROUP BY convert(char(9),datename(month,CreatedDate)) + ' '
+ convert(char(4),datepart(year,CreatedDate))
```
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/director-under-the-hood-new-users/monitordata-user_query.png" 
    alt="monitordata-user_query">

# In conclusion #
I hope this encourages you to take a look under the hood of Director to see what you can get out of it. The database infrastructure is much, much simpler than EdgeSight and should provide a lot of good detail.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*