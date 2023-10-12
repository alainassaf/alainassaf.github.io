---
layout: post
title: XenServer Drivers for WinPE
date: 2020-05-20
readtime: true
tags: [Automation, Virtualization, WinPE, XenServer]
thumbnail-img: /assets/img/xenserver-drivers-for-winpe/bernie.jpg
share-img: /assets/img/xenserver-drivers-for-winpe/bernie.jpg
---
![Bernie](/assets/img/xenserver-drivers-for-winpe/bernie.jpg)

<!-- wp:heading -->
<h2>Intro</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you deploy images with a Microsoft product, you are using WinPE (Microsoft Windows Preinstallation Environment) to configure and install an operating system. If you use XenServer for your Hypervisor of choice, then you will have to extract the XenServer drivers to import into your WinPE </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Extracting the Files</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The simplest way to get the files is to load the XenTools ISO onto a system and copy the install files over to a temporary location. Select your Virtual Machine, click the Console tab and select the DVD Drive drop down and select <code>guest-tools.iso</code>. In this case, my XenServer environment is on version 8.1.</p>
<!-- /wp:paragraph -->

![guesttools1](/assets/img/xenserver-drivers-for-winpe/guesttools.png)

<!-- wp:paragraph -->
<p>This will mount the current XenServer tools ISO onto the virtual machine's DVD drive.</p>
<!-- /wp:paragraph -->

![guesttools2](/assets/img/xenserver-drivers-for-winpe/guesttools2.png)

<!-- wp:paragraph -->
<p>To get the drivers, copy managementagentx64.msi (and managementagentx86.msi if you need drivers for a 32-bit operating system) to a temporary location on your system.</p>
<!-- /wp:paragraph -->

![guesttools3](/assets/img/xenserver-drivers-for-winpe/guesttools3.png)

<!-- wp:paragraph -->
<p>Here, we have the managementagentx64.msi file in the <code>D:\temp</code> folder. To extract the files we run the following command:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>msiexec.exe /a managementagentx64.msi /qb TARGETDIR="D:\temp\drivers"</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This runs installs <code>managementagentx64.msi</code> as an administrative install. This is why we used <code>/a</code> as a parameter and the <code>TARGETDIR</code> parameter so it will expand the MSI files into the <code>D:\temp\drivers</code> folder. I also used <code>/qb</code> to run the install in <strong>Q</strong>uiet mode with a <strong>B</strong>asic GUI. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>After running this command, we can now drill-down into folders created by the administrative install.</p>
<!-- /wp:paragraph -->

![guesttools4](/assets/img/xenserver-drivers-for-winpe/guesttools4.png)

<!-- wp:paragraph -->
<p>The MSI contains a lot of files and executables, but we're looking for drivers. Fortunately, there is a Drivers folder. Under Drivers there are v8 and v9 folders. v9 holds the newer drivers. Under v9 we have the folders that contain the driver files (both 64-bit and 32-bit versions).</p>
<!-- /wp:paragraph -->

![guesttools5](/assets/img/xenserver-drivers-for-winpe/guesttools5.png)

<!-- wp:heading -->
<h2>Importing the Drivers</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are five drivers that we will want to import into WinPE. In the following examples, I'm importing the drivers into Microsoft Deployment Tool.</p>
<!-- /wp:paragraph -->

![guesttools6](/assets/img/xenserver-drivers-for-winpe/guesttools6.png)

<!-- wp:paragraph -->
<p>I drill-down to my Deployment Share (MDT Production), expand Out-of-Box Drivers and right-click on WinPE 5.0 x64. The Import Driver Wizard start and we can drill down to the location of our drivers.</p>
<!-- /wp:paragraph -->

![guesttools7](/assets/img/xenserver-drivers-for-winpe/guesttools7.png)

<!-- wp:paragraph -->
<p>Click Next, Next and the import will begin. At the end a Confirmation window will display the status.</p>
<!-- /wp:paragraph -->

![guesttools8](/assets/img/xenserver-drivers-for-winpe/guesttools8.png)

<!-- wp:paragraph -->
<p>Since I didn't drill down into each driver folder, the wizard imported both the 32-bit and 64-bit versions of the drivers. I can delete the 32-bit versions by selecting them, right-clicking, and choosing delete.</p>
<!-- /wp:paragraph -->

![guesttools9](/assets/img/xenserver-drivers-for-winpe/guesttools9.png)

<!-- wp:paragraph -->
<p>Now to use the new drivers, you right-click on the Deployment share and select Update Deployment Share.</p>
<!-- /wp:paragraph -->

![guesttools10](/assets/img/xenserver-drivers-for-winpe/guesttools10.png)

<!-- wp:paragraph -->
<p>The Update Deployment Share Wizard comes up. My preference is to always choose "Completely regenerate the boot images." This takes longer, but the results are consistent.</p>
<!-- /wp:paragraph -->

![guesttools11](/assets/img/xenserver-drivers-for-winpe/guesttools11.png)

<!-- wp:paragraph -->
<p>The wizard will complete and a new ISO is generated in the Boot folder under your deployment share.</p>
<!-- /wp:paragraph -->

![guesttools12](/assets/img/xenserver-drivers-for-winpe/guesttools12.png)

<!-- wp:paragraph -->
<p>You copy this ISO to your XenServer ISO share, and when you want to build a new image, you point your XenServer virtual machine to this ISO and WinPE will use the new drivers you imported. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I hope this post helps you track down the XenServer drivers you need to automate builds in the future. Please comment if you have any questions.</p>
<!-- /wp:paragraph -->

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

<!-- wp:paragraph -->
<p><em>Thanks for reading,<br />Alain Assaf</em></p>
<!-- /wp:paragraph -->
