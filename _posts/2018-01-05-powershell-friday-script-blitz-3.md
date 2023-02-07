---
layout: post
title: PowerShell - Friday Script Blitz 3
date: 2018-01-05
readtime: true
tags: [Administration, PowerShell, Provisioning Services, Scripting]
thumbnail-img: /assets/img/powershell-friday-script-blitz-3/224edi.jpg
share-img: /assets/img/powershell-friday-script-blitz-3/224edi.jpg
---
![sirmixalot](/assets/img/powershell-friday-script-blitz-3/224edi.jpg)

# Intro #
Happy 2018!!! In my current position I’m getting to do a lot of PowerShell scripting. Typically these are quick scripts for maintenance or finding information about our Citrix environment. I’m posting several here to share.
<h6>NOTE: These scripts were written against a XenApp 7.9/PVS 7.15 environment</h6>

## get-pvsPersonalityStrings.ps1 ##
If you have to maintain unique personality strings on your provisioned devices, then this script will help. It spits out all the strings in a PVS farm. I wrote this script due to a requirement with Symantec AV that expects to see a unique hardware ID for servers connecting to it. One way to do this in provisioned environments is with a startup script. See <a href="https://techdocs.broadcom.com/us/en/ca-enterprise-software/business-management/clarity-client-automation/14-5/implementing/desktop-virtualization/updating-the-golden-template.html#concept.dita_a39d1c61cbe821ae3e2ca79e7af5ae68ff4ab4f7_ForXendesktopPVSConfigurethevDiskTargetDevicePersonalityData" target="_blank" rel="noopener">(For Xendesktop PVS) Configure the vDisk Target Device Personality Data</a> for more info.

## set-pvsPersonalityString.ps1 ##
The companion script for above. This script will set the Personality String for a PVS Device. It can take an input or generate one automatically. It assumes a 32 character hexadecimal string.

Both scripts are available on <a href="https://github.com/alainassaf/pvsScripts" target="_blank" rel="noopener noreferrer">GitHub</a>

<em>Thanks for reading,</em>
<em>Alain</em>
