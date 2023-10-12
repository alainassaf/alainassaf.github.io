---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 5
date: 2009-04-13
tags: [Business Intelligence, Citrix, Reporting Services, SQL, Visual Studio]
---
> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

To sum up how we got here:
<ol>
	<li>We used PowerShell to gather some specific session stats from Citrix MFCom and output them to a text file. [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-1/">part 1</a>] [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-2/">part 2</a>]</li>
	<li>We then created a database and table to hold this data. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3/">part 3</a>]</li>
	<li>Following this, we created a job in Visual Studio to parse the text file and insert it into the above table. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3/">part 3</a>]</li>
	<li>Then, I  demonstrated how to save the Visual Studio job directly to an MS SQL server and have that server run a job to insert new data on a periodic basis. [<a href="/2009-03-31-making-citrix-stats-work-for-you-part-4/">part 4</a>]</li>
</ol>

Now that we have new data automatically entering our database table every 5 minutes or so, we can construct a web page using MS SQL Reporting Services to present the data in real-time.  For this example, we'll create a real-time report that shows the currently disconnected users in my Citrix farm.

Open Visual Studio and open up a Report Server Project.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_1.jpg" 
    alt="stats5_1">

Now we'll create a Shared Data Source to connect to our database server by right-clicking on Shared Data Sources and Add New Data Source.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_2.jpg" 
    alt="stats5_2">

We're connecting to an MS SQL server, click Edit to enter the connection information, and choose the database we'll query for our data.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_3.jpg" 
    alt="stats5_3">

Now we'll add a report by right-clicking on Reports and (amazingly enough) 
select Add New Report
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_4.jpg" 
    alt="stats5_4">

This will bring up the Report Wizard which we'll follow for this example.  First, I'll use the data source we've already created.  Since it's already selected, we'll click Next and move on to the query builder.  Here, we'll create an SQL query that will gather the data we want to display in our report.  Click on Query Builder, then click on the Generic Query Designer button to bring up some QBE (query by example) tools.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_5.jpg" 
    alt="stats5_5">

Click on the Add Table button and select the table we want to query from.
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_6.jpg" 
    alt="stats5_6">

So, for this example, I want query this table to get the number of disconnected users currently in my Citrix farm.  So, I'm looking for a session state of 5, I just want to count each user once, and I only need the last 5 minutes since this is the update interval of the table.  Here's the query I'm using to get this data.

```sql
SELECT COUNT(DISTINCT CONVERT(varchar, UserName)) AS Disconnected
FROM UserSessions
WHERE (SessionState LIKE '%5%') AND (msgdatetime &amp;gt;= GETDATE() - .0031)
```

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_7.jpg" 
    alt="stats5_7">

This gives us the information we need, so I'll click OK and go back to the Query builder and click Next.  We'll choose Tabular for the report type, click Finish, and name the report.  This takes us to the design view, where we can format the report.  Hitting the Preview tab let us see how the finished report will look (I made some minor layout changes and added some text).
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-5/stats5_8.jpg" 
    alt="stats5_8">

For the final post in this series, we'll create a database view so we can show the currently disconnected users and the maximum disconnected users for the current day and upload the finished report to our Reporting Services server for public viewing.

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*