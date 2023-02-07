---
layout: post
title: Reporting on a user's bad experience
subtitle: EdgeSight
date: 2011-03-14
tags: [Business Intelligence, EdgeSight, MS SQL, MS SQL Reporting Services, Reporting, Monitoring, SQL]
---
![](/assets/img/icons/es-logo.jpg)

<strong>Intro (Soapbox)</strong>

I have stated it before, **EdgeSight** is one of your most powerful tools you have in your XenApp environment.  Tons of information is gathered and stored in the database that never sees the light of day because it’s difficult to get the information out with the built-in reports.  If you are a CIO/IT Manager/Team Lead you have to either develop the DBA/SQL skills of one of your XenApp administrators or form a workgroup with the DBA team to really leverage EdgeSight to benefit your customers.

In this post, we will create a query that summaries a user’s bad login experience and then use SSRS (SQL Server Reporting Services) to dill down to a more detailed report of just one user.

###### NOTE: Please review my series on *Making Citrix Stats Work for You* to get familiar with creating custom SQL queries and SSRS reports from those queries. 
###### Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

<strong>EdgeSight Under the Hood</strong>

My colleague, <a href="https://www.linkedin.com/in/packetjockey/" target="_blank"><b>John Smith</b></a>, has a terrific blog where he pulls back the veil on EdgeSight called <a href="https://edgesightunderthehood.wordpress.com/" target="_blank"><b>EdgeSight Under the Hood</b></a>.  After reading <a href="http://edgesightunderthehood.wordpress.com/2010/04/28/two-new-edgesight-queries-find-xendesktop-candidates-and-problem-users/" target="_blank">this</a> post, I put together a more detailed query on my user’s session startup experience.

<strong>User’s Bad Login Experience Query</strong>

```sql
SELECT [user] as 'Userid',
CAST(session_startup_server/1000.0 AS decimal(8,2))as 'Session Startup (sec)',
CAST(profile_load_server_duration/1000.0 as decimal(8,2)) as 'Profile Load (sec)',
CAST(credentials_obtention_server_duration/1000.0 as decimal(8,2)) as 'Obtain Creds (sec)',
CAST(login_script_execution_server_duration/1000.0 as decimal(8,2)) as 'Logon Script (sec)',
client_address as 'Client IP',
client_version as 'ICA Client Ver',
machine_name as 'Citrix Server',
CONVERT(varchar(30),DATEADD(hh,-5,start_time),0) as 'Session Start Time'
FROM vw_ctrx_archive_server_start_perf
WHERE DATEADD(hh,-5,start_time) &gt; dateadd(dd,-1,getdate()) and DATEADD(hh,-5,start_time) &lt; getdate()
GROUP BY session_startup_server, profile_load_server_duration, credentials_obtention_server_duration, login_script_execution_server_duration, client_address, client_version, machine_name, start_time, [user] having session_startup_server/1000.0 &gt; 60
ORDER BY 'Session Startup (sec)' desc, 'Userid'
```

As I stated above, EdgeSight records a lot of data and thankfully, Citrix combines most of the information in the form of views in the EdgeSight database.  The above query takes information from the VW_CTRX_ARCHIVE_SERVER_START_PERF view which contains user session startup information.  Specifically we are looking at the following data points (from the EdgeSight v5.3 help):

+ **session_startup_server** - This is the high level server connection startup metric. This includes the time spent on the Presentation Server performing the entire start-up operation. In the event of an application starting in a shared session, this metric is expected to be much smaller, as starting a completely new session involves potentially high cost tasks such as profile loading and login script execution.
+ **profile_load_server_duration** - The time spent on the server loading the users’ profile.
+ **credentials_obtention_server_duration** - The time taken for the server to obtain the user credentials. This is only likely to be a significant amount of time if manual login is being used and the server-side credentials dialog is displayed (or if a legal notice is displayed before login commences).
+ **login_script_execution_server_duration** - The time spent on the server running the users' login script(s).
+ **client_address** - Address of the client associated with the session.
+ **client_version** - Version of the client associated with the session.
+ **start_time** - The time when the session creation started

<strong>Here’s some example data:
</strong>
<table border="1" cellspacing="0" cellpadding="2" width="100%">
<tbody>
<tr>
<td width="44" valign="top">Userid</td>
<td width="44" valign="top">Session Startup (sec)</td>
<td width="44" valign="top">Profile Load (sec)</td>
<td width="44" valign="top">Obtain Creds (sec)</td>
<td width="44" valign="top">Login Script (sec)</td>
<td width="44" valign="top">Client IP</td>
<td width="44" valign="top">ICA Client Ver</td>
<td width="44" valign="top">Citrix Server</td>
<td width="44" valign="top">Session Start Time</td>
</tr>
<tr>
<td width="44" valign="top">user1</td>
<td width="44" valign="top">1236.25</td>
<td width="44" valign="top">23.31</td>
<td width="44" valign="top">1.03</td>
<td width="44" valign="top">1209.13</td>
<td width="44" valign="top">192.168.50.1</td>
<td width="44" valign="top">10.08.55362</td>
<td width="44" valign="top">CITRIX1</td>
<td width="44" valign="top">Mar 4 2011 4:48AM</td>
</tr>
<tr>
<td width="44" valign="top">user2</td>
<td width="44" valign="top">1222.83</td>
<td width="44" valign="top">14.17</td>
<td width="44" valign="top">1.09</td>
<td width="44" valign="top">1204.50</td>
<td width="44" valign="top">127.0.0.1</td>
<td width="44" valign="top">11.2.0.31560</td>
<td width="44" valign="top">CITRIX2</td>
<td width="44" valign="top">Mar 4 2011 2:45AM</td>
</tr>
</tbody>
</table>
First of all, the query is getting the ‘Session Start Time’ offset for Eastern Standard Daylight Savings Time (-5) with these 2 statements:

```sql
CONVERT(varchar(30),DATEADD(hh,-5,start_time),0) as 'Session Start Time'
```

and

```sql
WHERE DATEADD(hh,-5,start_time) &gt; dateadd(dd,-1,getdate()) and DATEADD(hh,-5,start_time) &lt; getdate()
```

The WHERE statement is set to give me all sessions that started from the last EdgeSight worker upload to 24 hours ago.  Also, EdgeSight returns times in milliseconds, so dividing the results by 1000 and ensuring we only display 2 decimal places gives us the seconds to perform each action.

Everyday you visit this SSRS report, you will get a list of users who have had terrible (as recorded by EdgeSight) login experiences, but how do you know if this was a fluke or due to the user connecting from an unusual location?

<strong>Drill baby drill!</strong>

One of the neat things available in SSRS is the ability to click on a result and use that data to generate a different report.  In other words, you can <em>drill-down</em> to a more detailed report.  This report should show the login history of the user.  That way we can tell if their bad experience is typical or was just a one-time problem.  We will modify the above query to display the same information, but we’ll remove the time restrictions from the WHERE clause to get more data.  We’re also dropping Userid from the table, because this query is about a specific user who we already know.

<strong>User’s Bad Login Experience Detail Query</strong>

```sql
SELECT CONVERT(varchar(30),DATEADD(hh,-5,start_time),0) as 'Session Start Time',
CAST(session_startup_server/1000.0 AS decimal(8,2))as 'Session Startup (sec)',
CAST(profile_load_server_duration/1000.0 as decimal(8,2)) as 'Profile Load (sec)',
CAST(credentials_obtention_server_duration/1000.0 as decimal(8,2)) as 'Obtain Creds (sec)',
CAST(login_script_execution_server_duration/1000.0 as decimal(8,2)) as 'Logon Script (sec)',
client_address as 'Client IP',
client_version as 'ICA Client Ver',
machine_name as 'Citrix Server'
FROM vw_ctrx_archive_server_start_perf
WHERE [user] = (@userid) and session_startup_server is not null and login_script_execution_server_duration &gt; 0
GROUP BY session_startup_server, profile_load_server_duration, credentials_obtention_server_duration, login_script_execution_server_duration, client_address, client_version, machine_name, start_time, [user]
ORDER BY 'Session Start Time' desc
```

This line is where we create a parameter called @userid. <em>Remember this for later.</em>

```sql
where [user] = (@userid) and session_startup_server is not null and login_script_execution_server_duration &gt; 0
```

So how do we create the drill-down?

<em>NOTE: The following screenshots are from Visual Studio 2005.</em>

In Visual Studio, open the report for the User’s Bad Login Experience Query.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image.png" width="636" height="346" alt="image">

Open the Layout tab. We want to use Userid as the parameter we pass to our drill-down report.  Right-click on the field and select properties.  You select the field and not the column name because we want to turn the results of the query into the parameter for the drill-down report.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image1.png" width="629" height="491" alt="image1">

In the Textbox Properties window, click on the Navigation Tab.  Under Hyperlink action, click “Jump to report:” and choose the BadLoginExpDetail report (<em>NOTE: BadLoginExpDetail is available because I had added it to the SSRS_User_Bad_Login_Exp project above</em>).

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image2.png" width="734" height="560" alt="image2">

Now click the Parameters… button.  When you click the drop-down arrow in the “Parameter Name” field, you will see userid.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image3.png" width="427" height="322" alt="image3">

Huh?  Yeah, look back at the **“User’s Bad Login Experience Detail Query”** above.  You will recall, that we created a parameter called `userid` by setting a condition in the `WHERE` clause that `[user] = @userid`.  That <a href="mailto:‘@’">‘@’</a> symbol is not just for show, but how you create parameters (variables) in SQL.  When you chose the BadLoginExpDetail report and clicked on the Parameter button, you could select any parameters you created in the query for that report.  Okay, now that we are on the same page, we have to associate a value with the parameter.  Click the drop-down arrow in the “Parameter Value” column.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image4.png" width="427" height="323" alt="image4">

You will see all the fields present in the BadLoginExp report.  We want to pick `“=Fields!Userid.Value”` so that when we click on a user’s name in the BadLoginExp report, it will send that name as the parameter to the BadLoginExpDetail report.  Now, click OK and OK again to close the properties box.  Let’s click on the Preview tab to see what happens.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image5.png" width="644" height="103" alt="image5">

As you can see, the user1 value is now a hyperlink that will pass user1 as the `‘userid’` parameter to the BadLoginExpDetail report.  When we click it, we get the following:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/edgesight-reporting-on-a-users-bad-experience/image6.png" width="644" height="92" alt="image6">

You can modify the userid field to make it look like a hyperlink by using underline and/or changing the text color.

I always welcome questions and comments.  Again, I cover the creation of SSRS reports in my Making Citrix Stats Work for you series. **Jump to** [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*