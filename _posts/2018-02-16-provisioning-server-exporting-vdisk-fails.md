---
layout: post
title: Citrix Provisioning Server - Exporting vDisk fails
date: 2018-02-16
readtime: true
tags: [Administration, Documentation, Provisioning Services, Troubleshooting]
thumbnail-img: /assets/img/provisioning-server-exporting-vdisk-fails/pvs-vdisk-export-you-have-failed-this-city.jpg
share-img: /assets/img/provisioning-server-exporting-vdisk-fails/pvs-vdisk-export-you-have-failed-this-city.jpg
---
![Arro](/assets/img/provisioning-server-exporting-vdisk-fails/pvs-vdisk-export-you-have-failed-this-city.jpg)

# Intro #
Provisioning Server is one of those workhorse products that works well day in and day out and then it doesn't. There's so many things that can go wrong for a number of reasons. This post covers a simple fix for when you can't export a vDisk.
# Versions #
There is a Mac/PC debate among Citrix engineers on whether MCS (Machine Creation Services) or PVS (Provisioning Services) is better. This post will NOT engage in this debate because clearly, PVS is better :).

One thing that distinguishes PVS from MCS, is vDisk versioning. With vDisk versions you can isolate updates to a vDisk or allow for quick revisions without affecting and copying a 30-40 GB file every time you want to make a change or update to your vDisk. For each version you can (and should) add notes to the Properties field.

![](/assets/img/provisioning-server-exporting-vdisk-fails/vdiskversionproperties1.png)

One thing we recently decided as a team was to utilize Bugzilla to track changes to our vDisks. Having a method for tracking changes is essential, especially if you have multiple team members and want everyone on the same page. As you can see above, I like to put a lot of info into the properties so that there's no question as to what changes the vDisk has. This unfortunately caused an issue recently as I was not able to export the vDisk to our other PVS farm.
# Export/Import #
When working with vDisk versions, exporting the vDisk is the easiest way to move those versions to another farm.

![](/assets/img/provisioning-server-exporting-vdisk-fails/vdiskexport.png)

You can select from what version you wish to export the vDisk and an XML file is generated. You then copy the XML and the AHVDX files to the destination PVS Store. From the destination Store, right-click and select "Add vDisk Versions..." and your vDisk versions are added.

# But... #
Normally, this works well in our environment, but this time I was not able to export the vDisk. I could bring up the export vDisk dialog and go through the motions, but no XML was generated. It turns out if you have multiple lines in the version description field, the export fails. You don't get any warning message or event log entry about it either so before you start going down the rabbit hole trying to find a fix. Double-check your properties field and make sure you have everything on one line. I hope Citrix resolves this in an upcoming PVS version, as it seems like a minor thing to fix.

![](/assets/img/provisioning-server-exporting-vdisk-fails/vdisktwolines.png)

![](/assets/img/provisioning-server-exporting-vdisk-fails/vdiskoneline.png)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*