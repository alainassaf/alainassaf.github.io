---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 3
date: 2009-03-27
tags: [Business Intelligence, Citrix, PowerShell, Scripting, Visual Studio]
---
> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

Okay put your propeller hats on ...

As I mentioned in my [last post](/2009-03-26-making-citrix-stats-work-for-you-part-2/), I am going to run the PowerShell script periodically to gather new data.  This is easily accomplished with a batch file and a Windows scheduled task.  Here's the batch file:

```
@echo off
powershell.exe -noninteractive w:\qfarm\Count-CitrixSession.ps1
exit
```

I'm going to run this batch file every 3 minutes with Windows Scheduled Tasks.  It will take a minute to run, so we'll get updated data about every 4 minutes.   To import this data, we will have to create a database/table to hold it.   For this case, I'm using an MS SQL 2005,  so I'll open Management Studio.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part1.jpg" 
    alt="bi-part1">

Next, we'll create a database and table (in this example, the database is already created).  The table will consist of a time/date stamp field and 4 fields that are from the output of the PoSH script.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part22.jpg" 
    alt="bi-part22">

The little bit of cleverness in this table lies with the **msgdatetime column**.  Its default value (which you set when you create the table) is the `getdate()` function in MS SQL.  That way, it will always get the current date and time that the row was created.  The other columns will be a plain text data type, which we will have to convert when we do certain queries.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part33.jpg" 
    alt="bi-part33">

Now we have the text file that's being updated every 4 minutes (approximately) and a table to hold the data.  I'm going to use Visual Studio 2005 to actually do the import.   Run VS and create a new Integration Services Project.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part4.jpg" 
    alt="bi-part4">

Now drag a Data Flow Task into your design window.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part5.jpg" 
    alt="bi-part5">

Double-click the new Data Flow Task and drag over a flat file source (to read the text files) and an OLE DB Destination (to communicate with the database).

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part6.jpg" 
    alt="bi-part6">

Double-click the Flat File Source and create a new Flat file connection manager.  Give the connection manager a name and click Browse to point it to the text file that the PoSH script is creating.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part7.jpg" 
    alt="bi-part7">

Now, we have to set the properties of the connection manager to correctly parse the text file.  This will allow easy import of the data into our database table.   In this case, we can use a semicolon as the column delimiter and {CR}{LF} as the row delimiter.  This gives us 4 columns with the username, the applicationame, the servername, and the session state.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part8.jpg" 
    alt="bi-part8">

Next, drag the green arrow from Flat File Source to OLE DB Destination.  Double-click on the OLE DB Destination and create a new OLE DB Connection manager.  Enter in the ODBC information to connect to the database and select the database we want to use.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part9.jpg" 
    alt="bi-part9">

Now we can select the table we created before.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part10.jpg" 
    alt="bi-part10">

Select Mapping to coorelate the fields in the text file with the fields in the table.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part11.jpg" 
    alt="bi-part11">

Now to test the data flow, click the green triangle in the tool bar or use the Debug menu and Start Debugging.  You should get confirmation that a number of rows were imported.  A quick query of the table in SQL can confirm that it has data.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-3/bi-part12.jpg" 
    alt="bi-part12">

Next post will deal with uploading the project to an SQL server and creating a schedule that will automatically run, thus importing new data into the database on a recurring schedule.

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 4\]](/2009-03-31-making-citrix-stats-work-for-you-part-4/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*