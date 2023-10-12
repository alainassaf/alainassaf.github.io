---
layout: post
title: The Ultimate citrix naming convention
date: 2019-05-31
tags: [Administration, Citrix, Documentation]
---
# Intro #
Implementing Citrix solutions continues to be complex and involves many components in a typical IT infrastructure. One of the things that may be overlooked in Citrix implementations is a standardized naming convention. In this post, which I intend to update as new Citrix components are added, I’m detailing my naming convention that I’ve used for a few years. My hope is that it will provide a naming framework for others to use as they see fit.

#### Servers ####
<table>
<tbody>
<tr>
<th colspan="3"><span style="font-weight:bold;">AABBB-C-DDDDD##</span></th>
</tr>
<tr>
<td><span style="font-weight:bold;">Category</span></td>
<td><span style="font-weight:bold;">Full Name</span></td>
<td><span style="font-weight:bold;">Abbreviation</span></td>
</tr>
<tr>
<td><span style="font-weight:bold;">AA</span>
Country Code</td>
<td>United States
Sweden
Singapore</td>
<td>US
SE
SI</td>
</tr>
<tr>
<td><span style="font-weight:bold;">BBB</span>
City Data Center</td>
<td>St. Louis
Stockholm
Singapore</td>
<td>STL
STO
SIN</td>
</tr>
<tr>
<td><span style="font-weight:bold;">C</span>
DTAP</td>
<td>Development
Test
User Acceptance
Production</td>
<td>D
T
A
P</td>
</tr>
<tr>
<td><span style="font-weight:bold;">DDDDD</span>
Server Role</td>
<td>AppDNA
Delivery Controller
Citrix Studio
Citrix Director
XenApp ZDC/STA
XenDesktop Studio
Provisioning Services
Web Interface
Storefront
Web Server
EdgeSight
Citrix Licensing
RDS Licensing
Admin
SQL
SQL Cluster
XenApp App Server
DFS
Domain Controller
User Profiles
App-V Sequencer
App-V Client (Test)
App-V Management
App-V Publishing
App-V Reporting
WEM Broker
Citrix Hypervisor
VMWare</td>
<td>ADNA
CTXDC
CTXST
CTXDR
XACTL
XDSTU
CTXPS
CTXWI
CTXSF
HTTP
EDSIT
CTXLC
RDSLC
ADMIN
SQL
SQLCL
XAAPP
DFS
DC
UPM
APPVS
APPVC
APPVM
APPVP
APPVR
WEM
CHV
ESX</td>
</tr>
<tr>
<td><span style="font-weight:bold;">##</span>
Server Number</td>
<td>First Server
Second Server</td>
<td>01
02</td>
</tr>
</tbody>
</table>
<h5>Desktops</h5>
<table>
<tbody>
<tr>
<th colspan="3"><span style="font-weight:bold;">AABBB-C-DDDEE##</span></th>
</tr>
<tr>
<td><span style="font-weight:bold;">Category</span></td>
<td><span style="font-weight:bold;">Full Name</span></td>
<td><span style="font-weight:bold;">Abbreviation</span></td>
</tr>
<tr>
<td><span style="font-weight:bold;">AA</span>
Country Code</td>
<td>United States
Sweden
Singapore</td>
<td>US
SE
SI</td>
</tr>
<tr>
<td><span style="font-weight:bold;">BBB</span>
City Data Center</td>
<td>St. Louis
Stockholm
Singapore</td>
<td>STL
STO
SIN</td>
</tr>
<tr>
<td><span style="font-weight:bold;">C</span>
DTAP</td>
<td>Development
Test
User Acceptance
Production</td>
<td>D
T
A
P</td>
</tr>
<tr>
<td><span style="font-weight:bold;">DDD</span>
Operating System</td>
<td>Windows 7
Windows 8
Windows 10
CenOS 5
Debian Wheezy 7</td>
<td>W7
W8
W10
C5
D7</td>
</tr>
<tr>
<td>EE
System Bit</td>
<td>x86 Processor
x64 Processor</td>
<td>32
64</td>
</tr>
<tr>
<td><span style="font-weight:bold;">##</span>
Desktop Number</td>
<td>First Desktop
Second Desktop</td>
<td>01
02</td>
</tr>
</tbody>
</table>
<h5>vDisks</h5>
<table>
<tbody>
<tr>
<th colspan="3"><span style="font-weight:bold;">AA-BBBBBCCDDEEEEFF-GG...GG-####</span></th>
</tr>
<tr>
<td><span style="font-weight:bold;">Category</span></td>
<td><span style="font-weight:bold;">Full Name</span></td>
<td><span style="font-weight:bold;">Abbreviation</span></td>
</tr>
<tr>
<td><span style="font-weight:bold;">AA</span>
Delivery Platform</td>
<td>XenApp
XenDesktop</td>
<td>XA
XD</td>
</tr>
<tr>
<td><span style="font-weight:bold;">BBBBB</span>
Operating System</td>
<td>Windows 7
Windows 8.1
Windows 10
Windows Server 2012
Windows Server 2016</td>
<td>W7
W8
W10
W2012
W2016</td>
</tr>
<tr>
<td><span style="font-weight:bold;">CC</span>
Service Release
(Optional)</td>
<td>Release 1
Release 2</td>
<td>R1
R2</td>
</tr>
<tr>
<td><span style="font-weight:bold;">DD</span>
Service Pack
(Optional)</td>
<td>Service Pack 1
Service Pack 2</td>
<td>S1
S2</td>
</tr>
<tr>
<td>EEEE
Build
(Optional)</td>
<td>Fall Creators Update
Creators Update</td>
<td>1709
1703</td>
</tr>
<tr>
<td>FF
System Bit</td>
<td>x86 Processor
x64 Processor</td>
<td>32
64</td>
</tr>
<tr>
<td>GG...GG
Short vDisk Description
(Optional)</td>
<td>vDisk for Office
vDisk for Accounting</td>
<td>Office
Account</td>
</tr>
<tr>
<td><span style="font-weight:bold;">####</span>
vDisk</td>
<td>Version 1 of the vDisk
Version 20 of the vDisk</td>
<td>0001
0020</td>
</tr>
</tbody>
</table>
<h5>Machine Catalogs</h5>
<table>
<tbody>
<tr>
<th colspan="3"><span style="font-weight:bold;">AA-B-CCCCCDDEE-FFFFFFF</span></th>
</tr>
<tr>
<td><span style="font-weight:bold;">Category</span></td>
<td><span style="font-weight:bold;">Full Name</span></td>
<td><span style="font-weight:bold;">Abbreviation</span></td>
</tr>
<tr>
<td><span style="font-weight:bold;">AA</span>
Delivery Platform</td>
<td>XenApp
XenDesktop</td>
<td>XA
XD</td>
</tr>
<tr>
<td><strong>B</strong>
DTAP</td>
<td>Development
Test
User Acceptance
Production</td>
<td>D
T
A
P</td>
</tr>
<tr>
<td><span style="font-weight:bold;">CCCCC</span>
Operating System</td>
<td>Windows 7
Windows 8.1
Windows 10
Windows Server 2012
Windows Server 2016</td>
<td>W7
W8
W10
W2012
W2016</td>
</tr>
<tr>
<td><span style="font-weight:bold;">DD</span>
Service Release
(Optional)</td>
<td>Release 1
Release 2</td>
<td>R1
R2</td>
</tr>
<tr>
<td>EE
Service Pack
(Optional)</td>
<td>Service Pack 1
Service Pack 2</td>
<td>S1
S2</td>
</tr>
<tr>
<td><span style="font-weight:bold;">FFFFFFF</span>
Description
(Optional)</td>
<td>Description of Machine Catalog if needed</td>
<td></td>
</tr>
</tbody>
</table>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*