---
layout: post
title: Network Based Applications
subtitle: Citrix Profiling Gotchas
date: 2011-06-13
tags: [Application, Citrix, Citrix Application Streaming, Virtualization]
thumbnail-img: /assets/img/icons/csp2.jpg
share-img: /assets/img/icons/csp2.jpg
---
<strong>Intro</strong>

I’ve been making the transition from Microsoft Application Virtualization (App-V) to Citrix Streaming recently and came across an issue involving an application that was on a network share.

This post will cover a work-around to stream an application residing on a network share.

<strong>Issue</strong>

We received the request to stream a client-server application.  The client install was straightforward with just a couple of DLL’s resident on the local system (Profiler).  I quickly found out that the client install must be initiated from the server, otherwise all the file locations were local instead of pointing to the server.  I also found that the install did not create a local EXE to launch from the Profiler, but instead assumed that you would create a shortcut from the server.  During the "profiling" (I think sequencing is a better word) I found that the Citrix Profiler is not aware of network locations and if I added a shortcut that pointed to the remote server and attempted to launch it, it would fail.

<strong>Solutions</strong>

I quickly turned to Google to find how to get around this issue. I suspect there is more than one way to skin a cat in this instance, but I used information I found in these 2 Citrix forum threads:

`http://forums.citrix.com/message.jspa?messageID=1390770`

`http://forums.citrix.com/message.jspa?messageID=1480938`

<strong>Work Around</strong>

I first tried to use the SUBST command to create a local mapping to the network share and perform the sequencing.  This failed to work in my case, so I created a VBS script that would be called by the published application.  In order to do this, I first created a shortcut to wscript.exe before I started the sequence.  I then added the shortcut to the profile after installing the application and used that shortcut as the published application in the stream.  When I did this, I set the additional command-line to ** (double asterisks).

When I published the streamed application, I put a UNC path to the following VBS script I wrote.

```
Set WshShell = WScript.CreateObject(&quot;WScript.Shell&quot;)
WshShell.CurrentDirectory = \\SERVER\SHARE\PROGRAMS
WshShell.Run \\SERVER\SHARE\PROGRAMS\APPLICATION.EXE
```

This configuration allows portions of the client application to be streamed, while still running the EXE from a network share.

Please review the above forum posts and as always I welcome any comments or questions.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*