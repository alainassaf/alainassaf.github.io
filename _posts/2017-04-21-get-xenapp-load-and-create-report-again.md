---
layout: post
title: Get XenApp Load and Create Report (Again)
date: 2017-04-21
readtime: true
tags: [PowerShell, Reporting, Monitoring, Scripting, XenApp]
thumbnail-img: /assets/img/get-xenapp-load-and-create-report-again/load2.jpg
share-img: /assets/img/get-xenapp-load-and-create-report-again/load2.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/get-xenapp-load-and-create-report-again/load2.jpg" 
    alt="mst3k">

# Intro #
Nostalgia is ruling movies and TV these days. <a href="https://www.netflix.com/title/80128275" target="_blank" rel="noopener"><b>Mystery Science Theater 3000</b></a> has returned from the dead to Netflix. I'm still getting through he first episode, so I'm still withholding judgement. In the spirit of going back to the well and rehashing old ideas, I've revisited my XenApp Load/Report script again.

###### Note: Since this blog post was written, MST3K has moved all it's content to the [Gizmoplex](https://mst3k.com/gizmoplex/about) #######

# Changes #
<ul>
	<li>I've moved the code to my <a href="https://github.com/alainassaf" target="_blank" rel="noopener"><b>Github</b></a> account</li>
	<li>I've removed the Logon Status column and replaced it with the server's worker group</li>
	<li>I've sorted the report by the Worker Group</li>
	<li>I fixed the formatting to display all the columns even if the first server was down. Before, if the first server queried by the script was down, then only the servername and status would show for all servers.</li>
</ul>

# The Report #
The report can be generated and sent to your browser of choice (the script defaults to Internet Explorer). In addition, you can set the SMTP information in the script have have it emailed.

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/get-xenapp-load-and-create-report-again/load1.png" 
    alt="load1">

# The Script #
Get the script from <a href="https://github.com/alainassaf/get-ctxloadandle" target="_blank" rel="noopener"><b>Github</b></a>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*