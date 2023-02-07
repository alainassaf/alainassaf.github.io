---
layout: post
title: E-mail Server Load and Assigned LE
subtitle: PowerShell
date: 2012-04-19 08:00
tags: [Citrix, PowerShell, Reporting/Monitoring, Troubleshooting, XenApp]
---
[**Note: I recently updated this script for XenApp 7.x environments.**](/2016-09-20-citrix-server-load-report/){:target="_blank"}

***Note: I wrote this script for a XenApp 5.0 environment with PowerShell 2.0 and CTP3 XenApp Cmdlets.***

> Update: I found that the simple ping used with Test-Connection PowerShell cmdlet was not identifying servers that were not available for login. In our environment we occasionally have a server come back from a scheduled reboot and ping, but not allow RDP/ICA connections
> I've incorporated a Test-Port function from [**Aaron Wurthmann's**](https://twitter.com/#!/awurthmann){:target="_blank"} [**PowerShell Script Connect-RDP.ps1**](https://github.com/nylyst/PowerShell/blob/master/Connect-RDP.ps1){:target="_blank"}

<strong>Intro</strong>  
Citrix has utilized load balancing in XenApp for years. This feature allows you to set parameters in a load evaluator, apply it to a server, and have Citrix manage how users are assigned to servers. Despite the numerous tools available on the market that report on your XenApp farm utilization, Citrix user load balancing remains the easiest to implement (and cheapest). That being said, beyond running the "qfarm /load" command over and over, it’s difficult to report on your farm’s load. Thankfully, with the Citrix PowerShell commands we can regularly report on a server’s load (including the load evaluator rules) and e-mail that information.

<strong>The Script</strong>  

```posh
###############################################################################
## Title : get-ctxLoadAndLE.ps1
## Description : Gathers server load, assigned LE, and active and disconnected sessions
## Author : Alain Assaf
## Date : 01/11/2012
## Sources : Logging and e-mail sections from Raido
## Sources : http://powershell.com/cs/blogs/ebook/archive/2008/10/23/chapter-4-arrays-and-hashtables.aspx
## Sources : http://technet.microsoft.com/en-us/library/ff730946.aspx
## Sources : http://technet.microsoft.com/en-us/library/ff730936.aspx
## Sources : [test-port function] http://irl33t.com/blog/2011/03/powershell-script-connect-rdp-ps1
## Notes : Assumes Citrix PowerShell cmdlets are installed
#### Changelog ################################################################
# -When - What - Who
# -01/11/2012 -Initial script -Alain Assaf/Chemtura
# -01/18/2012 - Changed way I get user sessions because it was timing out
# -02/20/2012 - Added sendTo variable to add mulitple receipients
# -03/05/2012 - Added lines to include LE Rules
# -04/26/2012 - Added Test-Port function from Aaron Wurthmann (aaron (AT) wurthmann (DOT) com)
###############################################################################

# Test-Port function to test RDP availability
# Written by Aaron Wurthmann (aaron (AT) wurthmann (DOT) com)
function Test-Port{
 Param([string]$srv=$strhost,$port=3389,$timeout=300)
 $ErrorActionPreference = "SilentlyContinue"
 $tcpclient = new-Object system.Net.Sockets.TcpClient
 $iar = $tcpclient.BeginConnect($srv,$port,$null,$null)
 $wait = $iar.AsyncWaitHandle.WaitOne($timeout,$false)
 if(!$wait)
 {
 $tcpclient.Close()
 Return $false
 }
 else
 {
 $error.Clear()
 $tcpclient.EndConnect($iar) | out-Null
 Return $true
 $tcpclient.Close()
 }
}
#Assign e-mail(s) to $sendto variable and SMTP server to $SMTPsrv
$sendto = SOMEONE@YOURDOMAIN.com
$SMTPsrv = 127.0.0.1

#Initialize array
$finalout = @()

#add XenApp Cmdlets
Add-PSSnapin -name *citrix* -erroraction silentlycontinue

#Get server list
$ctxservers = get-xaserver | select -Property servername | sort -Property servername

#Get user sessions
$ctxsrvSessions = Get-XASession | select -property SessionID,ServerName,AccountName,Protocol,State| where {($_.State -eq 'Active' -or $_.State -eq 'Disconnected') -and $_.Protocol -eq 'Ica'}

#Create a new object array with all the data we need
foreach ($srv in $ctxservers) {
 $ctxsrv = $srv.servername
 if (test-port $ctxsrv) {
  $srvload = get-xaserverload -servername $ctxsrv | select -Property ServerName,LoadEvaluatorName,Load,Rules
  $srvactive = @($ctxsrvSessions | where {$_.Servername -eq $ctxsrv -and $_.State -eq 'Active'}).count
  $srvdisconn = @($ctxsrvSessions | where {$_.Servername -eq $ctxsrv -and $_.State -eq 'Disconnected'}).count
  $objctxsrv = new-object System.Object
  $objctxsrv | Add-Member -type NoteProperty -name ServerName -value $srvload.servername
  $objctxsrv | Add-Member -type NoteProperty -name LEName -value $srvload.LoadEvaluatorName
  $objctxsrv | Add-Member -type NoteProperty -name Load -value $srvload.Load
  $objctxsrv | Add-Member -type NoteProperty -name Active -value $srvactive
  $objctxsrv | Add-Member -type NoteProperty -name Disconnected -value $srvdisconn
  $objctxsrv | Add-Member -type NoteProperty -name 'Server User Load' -value $srvload.Rules[0].Load.ToString()
  $objctxsrv | Add-Member -type NoteProperty -name 'Load Throttling' -value $srvload.Rules[1].Load.ToString()
  $objctxsrv | Add-Member -type NoteProperty -name 'Memory Usage' -value $srvload.Rules[2].Load.ToString()
  $objctxsrv | Add-Member -type NoteProperty -name 'CPU Usage' -value $srvload.Rules[3].Load.ToString()
  $finalout += $objctxsrv
 } else {
  $objctxsrv = new-object System.Object
  $objctxsrv | Add-Member -type NoteProperty -name ServerName -value $ctxsrv
  $objctxsrv | Add-Member -type NoteProperty -name LEName -value "OFFLINE"
  #$objctxsrv | Add-Member -type NoteProperty -name Load -value "0"
  #$objctxsrv | Add-Member -type NoteProperty -name Active -value "0"
  #$objctxsrv | Add-Member -type NoteProperty -name Disconnected -value "0"
  $finalout += $objctxsrv
 }
}

#Create HTML Header
$head = '<style>
META{http-equiv:refresh content:30;}
BODY{font-family:Verdana;}
TABLE{border-width: 1px;border-style: solid;border-color: black;border-collapse: collapse;}
TH{font-size:12px; border-width: 1px;padding: 2px;border-style: solid;border-color: black;background-color:Aquamarine}
TD{font-size:12px; border-width: 1px;padding: 2px;border-style: solid;border-color: black;background-color:GhostWhite}
</style>
<script type="text/javascript">
$(function(){
var linhas = $("table tr");
$(linhas).each(function(){
var Valor = $(this).find("td:first").html();
if(Valor == "OFFLINE"){
  $(this).find("td").css("background-color","Red");
}else if(Valor == "Running"){
  $(this).find("td").css("background-color","Green");
}
});
});
</script>
'
#$header = "<H5>XenApp DASHBOARD</H5>"
$title = "XenApp DASHBOARD"
#$body = $finalout | ConvertTo-Html -head $head -body $header -title $title
#$body = $finalout | ConvertTo-Html -head $head -title $title

#Create mail parameters
$messageParameters = @{
 Subject = "Report: Server Load and Assigned LE for XenApp Farm"
 Body = $finalout | ConvertTo-Html -head $head -title $title | out-string
 From = "Admin-Citrixtask@chemtura.com"
 To = $sendto
 SmtpServer = "$SMTPsrv"
}

#Send e-mail with Server load data
Send-MailMessage @messageParameters -BodyAsHtml

#Uncomment to output the results in an HTA file to view in a browser
#$finalout | ConvertTo-Html -head $head -title $title | out-file $env:temp\report.hta
#ii $env:temp\report.hta
```

<strong>Example e-mail</strong>

Currently the script records the following:
<ul>
 	<li>Server Name</li>
 	<li>Assigned Load Evaluator</li>
 	<li>Server Load</li>
 	<li>Active Sessions</li>
 	<li>Disconnected Sessions</li>
 	<li>Load Evaluator Rules (<em>you will have to modify the above script depending on which LE rules are using</em>)
<ul>
 	<li>User Load</li>
 	<li>Load Throttling</li>
 	<li>Memory Usage</li>
 	<li>CPU Usage</li>
</ul>
</li>
</ul>

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/powershell-e-mail-server-load-and-assigned-le/image22.png" alt="image">

I created a repeating scheduled task on a XenApp server in the farm I wanted to report on. Now my team gets an hourly report of the farm load.

<strong>Explore more</strong>
+ [Load Manager Values Explained](https://support.citrix.com/article/CTX111964){:target="_blank"}
+ [Load Manager Rules Explained](https://support.citrix.com/article/CTX105449){:target="_blank"}
+ [Performance Monitor Counters Used by Load Manager](http://support.citrix.com/article/CTX111965){:target="_blank"}
+ [Troubleshooting Load Balancing Issues](http://support.citrix.com/article/CTX112082){:target="_blank"}
+ [How to Enable the Load Manager Log for Load Balancing Issues](http://support.citrix.com/article/CTX112104){:target="_blank"}

<strong>Conclusion</strong>

Please try it out and let me know how it works for you. As you can see above, I borrowed a javascript that was supposed to color the cells for an OFFLINE server red, but have not been able to get it to work. If anyone has a solution, I will be happy to update this post and spotlight you, your blog, or twitter account.

As always, I welcome all comments and questions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*