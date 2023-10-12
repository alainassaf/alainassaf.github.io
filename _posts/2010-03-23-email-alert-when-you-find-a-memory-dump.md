---
layout: post
title: E-mail alert when you find a memory dump
subtitle: Wisdom-Fu
date: 2010-03-23
tags: [Administration, BSOD, memory dump, Reporting, Monitoring, Res Wisdom]
---
###### Note: This blog contains screenshots and references to an older version of [**Ivanti Automation**](https://www.ivanti.com/products/automation) which was developed by a company called **RES Software** that was aquired by Invanti.

~~RES Software~~ [**Ivanti**](https://www.ivanti.com/blog/ivanti-acquires-res) is probably best know for their ~~PowerFuse~~ [**Ivanti User Workspace Manager**](https://www.ivanti.com/products/user-workspace-manager) product which provides powerful and granular control of a system and a user's environment.  They also have a terrific product called ~~RES Wisdom~~ ~~RES Workspace Manager~~ [**Ivanti Automation**](https://www.ivanti.com/products/automation) which they describe as "Run Book  Automation for Windows."  

We utilize Wisdom every day to manage our XenApp farm and related servers.  I could spend pages and pages gushing about Wisdom, but I'm going to use this post to show how I use Wisdom to accomplish certain tasks. Naturally, the solution I present can be accomplished in a variety of ways, but I find Wisdom to be elegant and have a very short learning curve.  On top of that, it provides extensive,detailed change management and reporting which many products to not.

## Scenario

Typically, a XenApp environment has many, many variables at work that can compromise the stability of a system.  At times, this results in a crash dump and reviewing these dumps can give insight to what caused the problem.

NOTE: If you have a affinity for punishing yourself and want to actually dive into dumps, I highly recommend [**Crash Dump Analysis by Dmitry Vostokov**](https://www.dumpanalysis.org){:target="_blank"}.  Check out his minidump analysis series to get started.

###### Note: These links are now behind a paywall
+ [Minidump Analysis (Part 1)](https://www.dumpanalysis.org/blog/index.php/2007/08/29/minidump-analysis-part-1/){:target="_blank"}
+ [Minidump Analysis (Part 2)](https://www.dumpanalysis.org/blog/index.php/2007/09/06/minidump-analysis-part-2/){:target="_blank"}
+ [Minidump Analysis (Part 3)](https://www.dumpanalysis.org/blog/index.php/2007/09/12/minidump-analysis-part-3/){:target="_blank"}
+ [Minidump Analysis (Part 4)](https://www.dumpanalysis.org/blog/index.php/2007/10/11/minidump-analysis-part-4/){:target="_blank"}

In our environment, we have many servers and occasionally one will crash, reboot, and come back into production before we get an e-mail alert and we would not know if a dump was generated unless we connected to the server to find it.  I will describe how I created a Wisdom module to detect dump files on a server, copy them to a central location, and send an e-mail alert to the team.

<strong>Step 1 - Determine if a dump file exists</strong>

You can set up your server to create full and mini dumps by going into Computer properties, clicking the Advanced tab, and selecting Startup and Recovery.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image3.png" width="382" height="484" alt="image3">

This window will show you where the dump files are being created. Full memory dumps are written to: `%SystemRoot%\MEMORY.DMP` and minidumps are written to: `%SystemRoot%\Minidump\`

Here is the Wisdom Module that we’ll dive into:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image.png" width="631" height="484" alt="image">

The Execute Command task:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image1.png" width="631" height="484" alt="image1">

The command line is: `if not exist %WINDIR%\Minidump\*.dmp EXIT /B 1`

This is a conditional statement that looks for any file with a *.DMP extension in the Minidump directory.  If the file exists, then the command will successfully end with an error code of 0 (as set by Wisdom) otherwise it will fail and exit with an error code of 1.

<strong>Step 2 – E-mail someone that a memory dump file was found</strong>

The send e-mail task

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image2.png" width="631" height="484" alt="image2">

I’m highlighting the Condition portion of this task because this is where the conditional logic of the previous task (and its exit codes comes into play).  The condition is whether the previous task was successfully completed.  If so, the we set a DUMPEXISTS parameter to 1 if true or 0 if false.

<strong>Step 3 – Copy the memory dump to a network share and off the server</strong>

The perform file operations task

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image3.png" width="634" height="484" alt="image3">

The task creates a directory on a share (based on the server name – more on this later), copies the memory dump file to that location and then deletes it from the server.  The condition on this task is the value of DUMPEXISTS, which we set in the previous task.  If the e-mail task ran, then DUMPEXISTS is set to one, so this task will run and move the dump to a network share.

The remaining 3 tasks for this module repeat the previous 3 for the other memory dump type.

<strong>Wrapping Up</strong>

The SERVERNAME parameter (which is used when we copy the memory dump to the network) is simply formed by the %COMPUTERNAME% variable.  Wisdom, luckily, has access to the environment variables that are set on the machine the task is run on.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/email-alert-when-you-find-a-memory-dump/image4.png" width="627" height="484" alt="image4">

Finally, you should set this module to run on every reboot, then you’ll get e-mail alerts that memory dumps were generated if a server crashes.

<strong>Building Block</strong>

Wisdom allows you to export any resource, module, project or run book as an XML file that can be imorted to another Wisdom database.  Another wonderful feature.  I've sanitized a building block of this module for the community.  Due to WordPress file extension restrictions, I've renamed the building block with a .DOC extension.  Change it to .XML and you should be able to import it.

[**module_get dmp files if they exist**](/assets/img/module_get-dmp-files-if-they-exist.doc)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*