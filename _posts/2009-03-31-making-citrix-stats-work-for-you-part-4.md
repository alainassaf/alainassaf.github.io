---
layout: post
title: Making Citrix Stats Work for You
subtitle: Part 4
date: 2009-03-31
tags: [Business Intelligence, Citrix, SQL, Reporting Services, SQL, Visual Studio]
---
> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

To sum up how we got here:
<ol>
	<li>We used PowerShell to gather some specific session stats from Citrix MFCom and output them to a text file. [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-1/">part 1</a>] [<a href="/2009-03-26-making-citrix-stats-work-for-you-part-2/">part 2</a>]</li>
	<li>We then created a database and table to hold this data. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3">part 3</a>]</li>
	<li>Following this, we created a job in Visual Studio to parse the text file and insert it into the above table. [<a href="/2009-03-27-making-citrix-stats-work-for-you-part-3">part 3</a>]</li>
</ol>
Now, I will show how to save the Visual Studio job directly to an MS SQL server and have that server run a job to insert new data on a periodic basis.

With your Visual Studio package open, go to File and select Save Copy of Package As...

Select SQL Server for package location, enter the server name, set the authentication type and credentials for the DB owner.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-1.jpg" 
    alt="vs-1">

Choose and name a location for the Package Path and hit OK.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-2.jpg" 
    alt="vs-2">

Change the protection level to "Rely on server storage and roles for acces control" and click OK to save.

Now login the SQL server with Management Studio and open SQL Server Agent and Jobs.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-31.jpg" 
    alt="vs-31">

Right-click on jobs and select New Job.  Select the Steps page and click new.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-4.jpg" 
    alt="vs-4">

Hit the Type drop down and select SQL Server Integration Services Package.  Enter the server name and then look for the Package.  It will have the same name you gave it when you exported it to the SQL server.  Give the step a name and click OK.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-51.jpg" 
    alt="vs-51">

Now click on schedules to run this job every 5 minutes.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/making-citrix-stats-work-for-you-part-4/vs-6.jpg" 
    alt="vs-6">

Click OK, name the job and open the Job Activity Monitor to confirm the job kicks off.  If you get a Succeeded result, you can query the table to ensure it has got new data.  So now the SQL server is kicking off the Visual Studio data flow job to read a flat text file and enter the data into a table every 5 minutes.  Next post, we'll pull the data out of the table in some meaningful ways and place it in MS SQL Reporting Services.

> This post is part of a 6 part series.  Jump to [\[part 1\]](/2009-03-26-making-citrix-stats-work-for-you-part-1/)[\[part 2\]](/2009-03-26-making-citrix-stats-work-for-you-part-2/)[\[part 3\]](/2009-03-27-making-citrix-stats-work-for-you-part-3/)[\[part 5\]](/2009-04-13-making-citrix-stats-work-for-you-part-5/)[\[part 6\]](/2009-04-21-making-citrix-stats-work-for-you-part-6/)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*