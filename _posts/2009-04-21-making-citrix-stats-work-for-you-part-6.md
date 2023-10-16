---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 6
date: 2009-04-21
tags: [Business Intelligence, Citrix, Reporting Services, Reporting, Monitoring, SQL, Visual Studio]
---
> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)

To sum up how we got here:
<ol>
	<li>We used PowerShell to gather some specific session stats from Citrix MFCom and output them to a text file. [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-1/">part 1</a>] [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-2/">part 2</a>]</li>
	<li>We then created a database and table to hold this data. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3/">part 3</a>]</li>
	<li>We created a job in Visual Studio to parse the text file and insert it into the above table. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3/">part 3</a>]</li>
	<li>We showed how to save the Visual Studio job directly to an MS SQL server and have that server run a job to insert new data on a periodic basis. [<a href="/2009-03-31-making-citrix-stats-work-for-you-part-4">part 4</a>]</li>
	<li>We constructed a web page using MS SQL Reporting Services to present the data in real-time. [<a href="/2009-04-13-making-citrix-stats-work-for-you-part-5" target="_self">part 5</a>]</li>
</ol>
For this final post, I want to add another piece of information to my report (which I'll refer to as a dashboard from here on out).  I also will upload the report to the reporting services server so others can view the information.

We currently already show the currently disconnected users in the farm (last 5 minutes or so).  I also want to show the highest number of disconnected users for the day.  One quick way is to create a view based on our original data.  If we use the following query...

```sql
SELECT TOP (100) PERCENT CONVERT(varchar(5), msgdatetime, 108) AS Hour, COUNT(DISTINCT CONVERT(varchar, UserName))  AS Count
FROM dbo.UserSessions
WHERE (msgdatetime &gt;= (datediff(d,0,getdate()))) AND (CONVERT(varchar, SessionState) LIKE '%5%')
GROUP BY CONVERT(varchar(5), msgdatetime, 108)
ORDER BY Hour
```
...we get a listing of total, unique disconnected users ordered by every 5 minutes for the current day.  To create a view in MS SQL,  I'll open MS SQL Server Management Studio, login, open up our Syslog database and right-click on Views  and select "New View..."
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_1.jpg" 
    alt="stats6_1">

A view is just a persistent query.  I'll select the table we're going to use and plug in the above query.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_2.jpg" 
    alt="stats6_2">

Saving it will name the view and make it available to future queries.  Now we'll add a new field to our report that will show the largest number of disconnected users for the current day and base the information on our new view.  First, I'll add a new text box...
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_3.jpg" 
    alt="stats6_3">

and a new table object by clicking on Toolbox and dragging a table into the report.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_3b.jpg" 
    alt="stats6_3b">

Remove the footer and a couple of columns and do some editing...
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_3c.jpg" 
    alt="stats6_3c">

Now we'll add a new dataset by clicking on Data and selecting new Dataset on the drop down list.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_4.jpg" 
    alt="stats6_4">

I'll name the dataset MaxDisconnected and use the following query which just selects the maximum value (of disconnected sessions) from the view.

```sql
select max(count) as MaxDisconnected
from VW_UserSessState_5
```

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_51.jpg" 
    alt="stats6_51">

Now we have the new dataset. I'll click on Datasets and Layout and simply drag the MaxDisconnected dataset into the table I created earlier.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_6.jpg" 
    alt="stats6_6">

So, the report is complete.  I want this report to automatically update every 5 minutes.  Do to this, I'll click on the Report Menu and select Report Properties.  I'll simply click on Autorefresh and set it to 300 seconds...
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_7.jpg" 
    alt="stats6_7">

Now to import it to the reporting services server, we simply open our browser to the reporting services site:
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_8.jpg" 
    alt="stats6_8">

Click on Upload file and browse to the .RDL file.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_9.jpg" 
    alt="stats6_9">

Name the report and click OK.  Now the report will show up on the web page.  Clicking on it will result in a failure, because we still have to associate a data source.  Click on Properties and Data Sources.  Set the data source, click OK, hit apply, and then view.  The report will now render and will autoupdate every 5 minutes so will show the most recent data.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-6/stats6_10.jpg" 
    alt="stats6_10">

Thanks for viewing.  I hope this gives you a basic introduction to using several tools to present data in a simple way (even though the method may be complicated).  As a system administrator/engineer  you (hopefully) typically have a reporting tool of some sort, but giving superiors or other teams access to it may prove to be more difficult or confusing than intended.  Using SQL Reporting Services allows you to show data in a simple form to a wider audience.

Please feel free to ask questions/comment.  The methods described in this series have worked well in our environment, but I'd love to hear what others are doing to place metrics, stats, and such front and center.

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*