---
layout: post
title: Documenting Citrix Policies
subtitle: With PowerShell
date: 2010-05-12
tags: [Citrix, Documentation, PowerShell, Scripting, XenApp]
---
<em><strong>UPDATE: I had updated this script (updated link in the comments below), but had not updated the post with the new script. This has been done.</strong></em>

<em>Disclaimer: This post references PowerShell XenApp Commands that were released as a technology preview for XenApp 4.5/5.0. I have not tested the script against the XenApp 6 which uses a different version of these XenApp Commands.
</em>

Documentation is a vital (yet rarely loved) part of systems administration/engineering.  Exhaustively documenting Citrix can turn your hair white.  There are  farm-wide settings, polices, application publishing properties, zone configuration, and that’s just the beginning.

We are in the midst of architecting a new Citrix environment where I work and we are taking advantage of this to review everything we have done before and changing it if needed.  We’re also attempting to document every facet of our new environment which includes, Provisioning (DHCP), XenApp, XenServer, XenDesktop, AppSense, Netscalers, Web Interface, and MS App-V.

For this post, I’m providing a PowerShell script I modified from <a href="https://web.archive.org/web/20120121065812/http://kentfinkle.com:80/default.aspx" target="_blank">Kent Finkle</a> that will capture a Citrix Policy and what it’s applied to in a Word document.  The script will also export the Citrix policy and its filter (what it’s applied to) to 2 XML files that can be used to recreate or restore the configuration if it’s lost.

NOTE: This script assumes that the XenApp PowerShell Commands are installed on the server you’re running the script from.  You can download them from [**Citrix.com**](https://www.citrix.com){:target="_blank"}

Here’s the script:

```posh
#============================================================================
# NAME: get-citrixpolicy.ps1
# AUTHOR: Alain Assaf
# DATE  : 5/11/2010
#
# SOURCE1: Author: Kent Finkle http://kentfinkle.com/CreateSaveWordDoc.aspx
# SOURCE2: Author: Mark Alexander Bain http://command-line-programming.suite101.com/article.cfm/how_to_create_a_word_document_with_powershell
# COMMENT: Output a Citrix Policy to a Word document.
#          Assumes XenApp Commands are installed on source server
# VERSION: 1.0.0 - Initial script
# VERSION: 1.0.5 - Added policy filter and xml export of policy and filter
# VERSION: 1.0.6 - 8/23/2010 - Added prompt for policy and got document to
#                  automatically save and close.
#============================================================================
#Load XenApp Commands
Add-PSSnapin -Name *citrix*

#Initalize farm
$farm = get-xafarm

#Output list of Current Policies and prompt for one to create a document and backup
Get-XAPolicyConfiguration | select PolicyName
$polname = Read-Host "Enter a Citrix Policy to create a report and backup"

#Set variables
$docpath = "\\NETWORKSHARE\Documentation\Architecture\Citrix\policies"
$hname = $env:computername
$uname = $env:username
$a = get-date –format g
$b = get-date -uformat "%m%d%Y"
$oMissing = [System.Reflection.Missing]::Value

#Test path &amp; create if not present
if (!(Test-path -path $docpath)) { new-item $docpath -type directory | out-null }

# Create new Word document
$objWord = New-Object -comobject Word.Application
$objWord.Visible = $True
$objWord.Activate()

$objDoc = $objWord.Documents.Add($oMissing, $oMissing, $oMissing, $oMissing)
$objSelection = $objWord.Selection

$objSelection.Font.Name = "Arial"
$objSelection.Font.Size = "18"
$objSelection.TypeText("Citrix Policy Report")
$objSelection.TypeText(" for Farm: " + $farm.FarmName)
$objSelection.TypeParagraph()
$objSelection.Font.Size = "8"
$objSelection.Font.Italic = $True
$objSelection.TypeText("Script run on: " + $hname)
$objSelection.TypeText(" by: " + $uname)
$objSelection.TypeText(" at " + $a)
$objSelection.Font.Italic = $False
$objSelection.TypeParagraph()

$objSelection.Font.Size = "10"
$policy = Get-XAPolicyConfiguration -PolicyName $polname
$objSelection.Font.Bold = $True
$objSelection.TypeText("Citrix Policy: " + $policy.PolicyName)
$objSelection.Font.Bold = $False
$objSelection.Font.Size = "10"

$outpol = $policy | Out-String
$objSelection.TypeText("" + $outpol)
#$objSelection.TypeParagraph()

$policyfilter = Get-XAPolicyFilter -PolicyName $polname
$objSelection.Font.Bold = $True
$objSelection.TypeText($policyfilter.PolicyName + " policy applied to:")
$objSelection.Font.Bold = $False
$objSelection.Font.Size = "10"

$outpolfilter = $policyfilter | Out-String
$objSelection.TypeText("" + $outpolfilter)
#$objSelection.TypeParagraph()

$doctitle = $farm.FarmName + "_" + $policy.PolicyName + "_" + $b
$savepath = "$docpath\$doctitle.doc"
$objDoc.SaveAs($savepath,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing,$oMissing)
$objDoc.Close()
$objWord.Quit()

$poltitle = $farm.FarmName + "_" + $policy.PolicyName + "_Policy" + "_" + $b
$polfiltertitple = $farm.FarmName + "_" + $policyfilter.PolicyName + "_PolicyFilter" + "_" + $b

export-clixml -path "$docpath\$poltitle.xml" -InputObject $policy
export-clixml -path "$docpath\$polfiltertitple.xml" -InputObject $policyfilter
```

Here's a sample of the Word document (sanitized for public consumption):

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;"
    src="/assets/img/documenting-citrix-policies/image.png" width="408" height="484" alt="image">

The intention is to run this script periodically to provide documentation and a backup of all the policies applied to a farm.  I encourage you to explore the PowerShell commands provided by Citrix.  You will be able to document every aspect of your farm and also have an easy way to backup/restore the information as needed.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*