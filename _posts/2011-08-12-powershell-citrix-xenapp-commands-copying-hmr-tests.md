---
layout: post
title: Copying Health Monitoring and Recovery Tests
subtitle: PowerShell and Citrix XenApp Commands
date: 2011-08-12
tags: [Administration, Citrix XenApp, Microsoft, PowerShell, Scripting, XenApp]
---
<strong>Intro</strong>

Your Citrix farm will have the Health Monitoring and Recovery Tests turned on for all servers by default.  I have found that some tests can cause problems in the daily operation of your farm.  In particular, the XML Service Test will take your XenApp servers out of production since its default action is to remove the server from load balancing if the test fails. Typically you have to run the following command to discover if you have any servers removed this way (unless you love watching the alerts section in the AMC):

```
QFARM /LBOFF
```

This command will place the server (CTXSERVER1) back into load balancing:

```
ENABLELB CTXSERVER1
```

My advice is to only run the XML Service Test on the servers you have dedicated to XML ticket requests, and if you happen to have a NetScaler I would manage your XML load balancing with it instead of using the HMR tests.  In this post, I will cover some XenApp PowerShell cmdlets that will allow you to remove this test from all the servers in your farm.

<strong>PowerShell</strong>

First, get your current HMR tests and save them in a variable (NOTE: You should have the Citrix Cmdlets loaded):

```posh
Add-PSSnapin -Name Citrix.XenApp.Commands
$HMRTests = Get-XAHmrTest
```

<span style="font-family:Calibri;">Which gives us:</span>

```
PS C:\&gt;$HMRTests
TestName : Citrix IMA Service test
Description : This test queries the service to ensure that it is running by enumerating the applications available on the server.
FilePath : Citrix\IMATest.exe
Arguments :
Interval : 60
Threshold : 5
Timeout : 60
RecoveryAction : AlertOnly
ServerName :

TestName : Logon Monitor test
Description : Logon/logoff cycles are monitored to determine whether there is a problem with session initialization or possibly an application failure. If there are a lot of short cycles within a short time period, a problem is assumed to exist
FilePath : Citrix\LogonMonitor.dll
Arguments : /SessionTime:5 /SessionThreshold:50 /SampleInterval:600
Interval : 1
Threshold : 5
Timeout : 1
RecoveryAction : AlertOnly
ServerName :

TestName : Terminal Services test
Description : This test enumerates the list of sessions running on the server and the session user information, such as user name.
FilePath : Citrix\CheckTermSrv.exe
Arguments :
Interval : 60
Threshold : 5
Timeout : 30
RecoveryAction : AlertOnly
ServerName :

TestName : XML Service test
Description : This test requests a ticket from the XML service running on the server and prints the ticket.
FilePath : Citrix\RequestTicket.exe
Arguments :
Interval : 60
Threshold : 5
Timeout : 60
RecoveryAction : RemoveServerFromLoadBalancing
ServerName :
```

<span class="Apple-style-span" style="font-family:Calibri;font-size:13px;line-height:19px;white-space:normal;">So we have an object-array full of the HMR tests, but we don’t want the last one, namely the XML Service Test. I’m going to create a new object array variable that will only hold the first 3 objects in $HMRTests.</span>

```posh
$HMRNewTests = $HMRTests[0..2]
```

The default Farm Properties for the Heath Monitoring and Recovery are to Use Farm Settings.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-citrix-xenapp-commands-copying-hmr-tests/image4.png" width="644" height="267" alt="image4">

Server Properties:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-citrix-xenapp-commands-copying-hmr-tests/image5.png" width="644" height="225" alt="image5">

In order to turn this off you can use the following command:

```posh
Get-XAServer -ServerName CTXSERVER1 | Set-XAServerConfiguration -HmrUseFarmSettings $false -HmrEnabled $true
```

This changes the server properties to:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-citrix-xenapp-commands-copying-hmr-tests/image6.png" width="644" height="224" alt="image6">

Now you just have to copy the HMR Tests to the server with this command:

```posh
$HMRNewTests | New-XAHmrTest -ServerName CTXSERVER1
```

Now the server properties shows the new tests without the XML Service Test:

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-citrix-xenapp-commands-copying-hmr-tests/image7.png" width="644" height="268" alt="image7">

Using these commands you can make the changes to all the servers in your farm at once, but please test in a development environment first and use the handy –whatif flag on your commands to confirm the changes are what you intend.

As always, I welcome all comments and questions (especially about a better way to do this with PowerShell).

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*