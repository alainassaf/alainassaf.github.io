---
layout: post
title: Add PowerShell to Windows Context Menu
date: 2017-12-11
tags: [Administration, PowerShell, Scripting]
thumbnail-img: /assets/img/add-powershell-to-windows-context-menu/holiday-hacks.jpg
share-img: /assets/img/add-powershell-to-windows-context-menu/holiday-hacks.jpg
---
![holidayhack](/assets/img/add-powershell-to-windows-context-menu/holiday-hacks.jpg)

Here’s a holiday hack for PowerShell. If you would like the ability to right-click on a directory and have PowerShell open to that directory here’s what you do…
<ol>
	<li>Open the registry on the system you want to do this on (<B>regedit.exe</B>)</li>
	<li>Navigate to <code>HKEY_CLASSES_ROOT\directory\shell</code></li>
</ol>

![image1](/assets/img/add-powershell-to-windows-context-menu/image-001.png)

<ol start="3">
	<li>Right-click and create a new key called PowerShellPrompt</li>
</ol>

![image2](/assets/img/add-powershell-to-windows-context-menu/image-002.png)

<ol start="4">
	<li style="text-align:left;">Rename the Default Data to “PoSH Here” or something similar. This is what will display in the context menu when you right-click</li>
</ol>

| ----------- | ----------- |
| ![image3](/assets/img/add-powershell-to-windows-context-menu/image-003.png) | ![image4](/assets/img/add-powershell-to-windows-context-menu/image-004.png) |

<ol start="5">
	<li>Right-click on the PowerShellPrompt key and create a new key. Name it Command</li>
</ol>

![image5](/assets/img/add-powershell-to-windows-context-menu/image-005.png)

<ol start="6">
	<li>Change the Default data to the following command-line</li>
</ol>

```posh
powershell.exe -noprofile start-process  powershell.exe -verb runas -argumentlist "{ -noprofile -noexit cd %1}
```

![image6](/assets/img/add-powershell-to-windows-context-menu/image-006.png)

<ol start="7">
	<li>Now, when you right-click on a directory, An elevated PowerShell prompt will open at that directory</li>
</ol>

![image9](/assets/img/add-powershell-to-windows-context-menu/image-009.png)

<ol start="8">
	<li>If you want to load your profile when this prompt opens, remove the second “-noprofile” from the command-line</li>
</ol>
```posh
powershell.exe -noprofile start-process  powershell.exe -verb runas -argumentlist "{ -noexit cd %1}"
```

<ol start="9">
	<li>If you want to have an Icon show up in the Context menu add the following String value to the PowerShellPrompt key</li>
</ol>

![image11](/assets/img/add-powershell-to-windows-context-menu/image-011.png)
![image12](/assets/img/add-powershell-to-windows-context-menu/image-012.png)

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*