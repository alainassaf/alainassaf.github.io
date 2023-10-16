---
layout: post
title: WEM - Notes from the Field 2
date: 2017-07-11
readtime: true
tags: [Active Directory, Troubleshooting, Workspace Environment Manager, Workspace Management]
thumbnail-img: /assets/img/wem-notes-from-the-field-2/whatifitoldyou.jpg
share-img: /assets/img/wem-notes-from-the-field-2/whatifitoldyou.jpg
---
![morpheus](/assets/img/wem-notes-from-the-field-2/whatifitoldyou.jpg)

# Quick Note #
If you are using group policy (and why wouldn't you) to configure your WEM agents, pay close attention to leading and/or trailing spaces. This has happened twice to me with agent versions 4.2 and 4.3.

The <em>Connection Broker Name</em> and <em>Site Name</em> fields in the <em>Workspace Environment management\Agent Host Configuration</em> administrative template must <strong>NOT</strong> contain any leading or trailing spaces. If you have any, then the agent cannot communicate with your WEM Broker server nor can it load the correct Configuration Site.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*