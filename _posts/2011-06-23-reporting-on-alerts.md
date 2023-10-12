---
layout: post
title: Reporting on Alerts
subtitle: EdgeSight
date: 2011-06-23
tags: [Citrix, EdgeSight, MS SQL, MS SQL Reporting Services, Reporting, Monitoring, SQL]
---
<a href="https://citrix.com/" target="_blank"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;float:left;padding-top:0;border:0;margin:0 15px 0 5px;" title="es-logo" src="/assets/img/icons/es-logo.jpg" alt="es-logo" align="left" border="0" />

**EdgeSight** allows to you to create alerts that trigger on many criteria.  In this post, we will configure an alert and show how to query the database directly to get this information.

<strong>Creating an alert</strong>  
For the purposes of this post, I have created a Process Hung alert for outlook.exe.  This is a built-in Application Error alert that can trigger on the EXE file name, the application description, the process file version, and/or the process company name.  The actual alert will show up in the Farm Monitor and Alert List view under the Monitor Tab in the EdgeSight console.

Now you will get a near real-time alert in the console that looks like this:
<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image8.png"  width="800" height="88" alt="image8">

I found that this alert triggered quite often and while you can use the “Process Not Responding Alert” report, this blog is all about pulling back the veil.

<strong>The Query</strong>

We will use the VW_ES_ARCHIVE_ALERT view for this query.  Here is an example of all the columns in this view (customer specific information hidden):

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image9.png" 
     width="859" height="80" alt="image9">

For our purposes, I want to get the date of the alert, the machine name, the username, the process name, the process description, and the actual text of the alert.

```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
SELECT DISTINCT CONVERT(VARCHAR,time_stamp,111) AS 'Date', machine_name, account_name, exe_name, alert_name,alert_text
FROM vw_es_archive_alert
WHERE alert_name = 'Process Hung'
and exe_name = 'Outlook.exe'
and CONVERT(VARCHAR,time_stamp,111) = CONVERT(VARCHAR,@mydate,111)
```

This gives me:  
<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image10.png" 
     width="554" height="116" alt="image10">

If you look at the alert_text field, you will see some information that doesn’t look right.  You can see “Microsoft Office Outlook”, a weird character, and a series of numbers.  These numbers are in fact the actual process hang measured in milliseconds.  You can see this if you go back to the farm monitor and select the detail for an alert:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image11.png"  width="313" height="127" alt="image11">

You have the information you need to determine who is having a real long delay, but how can we sort or organize this delay information.  There is no built-in MSSQL function to break this column up into two useful fields.  A <a href="http://stackoverflow.com/questions/1007697/how-to-strip-all-non-alphabetic-characters-from-string-in-sql-server" target="_blank">Google search</a> pointed me to a user-written function that will strip non-alphanumeric from a column.

```sql
CREATE FUNCTION [dbo].[fn_StripCharacters]
(
 @String NVARCHAR(MAX),
 @MatchExpression VARCHAR(255)
)
RETURNS NVARCHAR(MAX)
AS
BEGIN
 SET @MatchExpression = '%['+@MatchExpression+']%'
 WHILE PatIndex(@MatchExpression, @String) &gt; 0
 SET @String = Stuff(@String, PatIndex(@MatchExpression, @String), 1, '')
 RETURN @String
END
```

Once you execute this in the MSSQL Management Studio, you can reference the function in your query:

```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
SELECT DISTINCT CONVERT(VARCHAR,time_stamp,111) AS 'Date', machine_name, account_name, exe_name, alert_name, dbo.fn_StripCharacters(alert_text, '^a-z0-9')
FROM vw_es_archive_alert
WHERE alert_name = 'Process Hung'
and exe_name = 'Outlook.exe'
and CONVERT(VARCHAR,time_stamp,111) = CONVERT(VARCHAR,@mydate,111)
```

This now gives us:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image12.png"  width="547" height="155" alt="image12">


Now the special character is gone, but how can you split the process delay out of the column?  You can use a built-in MSSQL function call SUBSTRING.

```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
SELECT DISTINCT CONVERT(VARCHAR,time_stamp,111) AS 'Date', machine_name, account_name, exe_name, alert_name, SUBSTRING(dbo.fn_StripCharacters(alert_text, '^a-z0-9'),23,6) AS 'Delay'
FROM vw_es_archive_alert
WHERE alert_name = 'Process Hung'
and exe_name = 'Outlook.exe'
and CONVERT(VARCHAR,time_stamp,111) = CONVERT(VARCHAR,@mydate,111)
```

Now we get:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image13.png"  width="441" height="157" alt="image13">

To finish up, we’ll divide the Delay by 1000 to get the delay in seconds.

```sql
DECLARE @mydate DATETIME
Set @mydate = GETDATE()
SELECT DISTINCT CONVERT(VARCHAR,time_stamp,111) AS 'Date', machine_name, account_name, exe_name, alert_name, CONVERT(INTEGER,SUBSTRING(dbo.fn_StripCharacters(alert_text, '^a-z0-9'),23,6),10)/1000.0 AS 'Delay'
FROM vw_es_archive_alert
WHERE alert_name = 'Process Hung'
and exe_name = 'Outlook.exe'
and CONVERT(VARCHAR,time_stamp,111) = CONVERT(VARCHAR,@mydate,111)
ORDER BY 'Delay' desc
```

Our end result:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/reporting-on-alerts/image14.png"  width="460" height="156" alt="image14">


With this information, you can do further manipulation including counting the number of alert instances for a user or tracking a single user over time.

As always I welcome all questions and comments.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*