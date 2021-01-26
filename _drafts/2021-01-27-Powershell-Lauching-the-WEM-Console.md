---
layout: post
title: "Powershell: Lauching the WEM Console"
subtitle: Open the WEM Console and connect to multiple WEM databases.
date: 2021-01-27
tags: [PowerShell,WEM]
bigimg:
    - "/img/2021-01-27-Lauching the WEM Console.jpg" : "Pixabay"
---

<!--more-->

# Index

* TOC
{:toc}

# Senario


# Open-WEMConsole

# The code
```posh
<#
.SYNOPSIS
Opens WEM Mgmt Console connected to entered WEM Infrastrucure Server
.DESCRIPTION
Opens WEM Mgmt Console connected to entered WEM Infrastrucure Server
.PARAMETER WEMServer
Optional string parameter. The WEM Infrastrucure server the WEM Console will connect to.
.EXAMPLE
PS> open-WemConsole.ps1
Opens the WEM Console connected to the last WEM infrastrcuture server.
.EXAMPLE
PS> open-WemConsole.ps1  -wemserver WEMSERVER
Updates the registry with WEMSERVER and opens the Console connected to WEMSERVER.
.EXAMPLE
PS> open-WemConsole.ps1  -wemserver WEMSERVER -verbose
Updates the registry with WEMSERVER and opens the Console connected to WEMSERVER. Provides additional verbose feedback.
.INPUTS
None
.OUTPUTS
None
.NOTES
NAME: open-WemConsole.ps1
VERSION: 1.00
CHANGE LOG - Version - When - What - Who
1.00 - 10/30/2020 - Initial script - Alain Assaf
AUTHOR: Alain Assaf
LASTEDIT: October 30, 2020
.LINK
http: //www.linkedin.com/in/alainassaf/
#>
#requires -modules NetTCPIP
[CmdletBinding()]
param (
    [Parameter()]
    [ValidateScript( { Test-NetConnection -ComputerName $_ -Port 8284 -InformationLevel Quiet })]
    [String]$WEMServer
)

#Constants
$datetime = Get-Date -Format "MM-dd-yyyy_HH-mm"
$Domain = (Get-ChildItem env:USERDNSDOMAIN).value
$ScriptRunner = (Get-ChildItem env:username).value
$compname = (Get-ChildItem env:COMPUTERNAME).value
$scriptName = $MyInvocation.MyCommand.Name
$scriptpath = $MyInvocation.MyCommand.Path
$currentDir = Split-Path $MyInvocation.MyCommand.Path

$ConsolePath = 'C:\Program Files (x86)\Norskale\Norskale Administration Console\Norskale Administration Console.exe'
$registryPath = "HKCU:\SOFTWARE\VirtuAll Solutions\VirtuAll User Environment Manager\Administration Console"
$Name = "LastBrokerSvcName"
$value = $WEMServer

if ($WEMServer -eq "") {
    $WEMServer = ((Get-ItemProperty -Path $registryPath -Name $name).LastBrokerSvcName).tostring()
} else {
    if (!(Test-Path $registryPath)) {
        Write-Warning "[$registryPath] not found. Confirm WEM Console is installed."
        Exit 1
    } else {
        try {
            Set-ItemProperty -Path $registryPath -Name $Name -Value $value
        } catch {
            Write-Warning "Failed to change [$registryPath\$Name]"
            Exit 1
        }
    }
}

Write-Host "Starting WEM Console connected to [$WEMServer]"
Start-Process $ConsolePath

#Script info
Write-Verbose "SCRIPT NAME: $scriptName"
Write-Verbose "SCRIPT PATH: $scriptPath"
Write-Verbose "SCRIPT RUNTIME: $datetime"
Write-Verbose "SCRIPT USER: $ScriptRunner"
Write-Verbose "SCRIPT SYSTEM: $compname.$domain"
```


# Conculsion


# Learning More


*Thanks for Reading,*

*Alain Assaf*
