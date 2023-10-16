---
layout: post
title: XenServer - Change Root Password
date: 2017-06-16
readtime: true
tags: [Administration, XenServer, Citrix Hypervisor]
thumbnail-img: /assets/img/xenserver-change-root-password/keep-calm-and-change-your-xenserver-password-3.png
share-img: /assets/img/xenserver-change-root-password/keep-calm-and-change-your-xenserver-password-3.png
---
![keepcalm](/assets/img/xenserver-change-root-password/keep-calm-and-change-your-xenserver-password-3.png)

# Intro #
Your boss comes to you in a panic about security and passwords. You sip your coffee and calmly let her vent. You assure her that yes, you can quickly and easily change the root password on all your XenServers. She walks away confident you know what you are talking about.
<h1>Change that password...or can you?</h1>
You hit the Internet for information on changing the XenServer root password and are hit with article after article about recovering a lost root password. That doesn't apply to you. You have your root password safely stored in your password store (right :)).
<ul>
	<li><a href="http://support.citrix.com/article/CTX116019" target="_blank" rel="noopener">How to Reset Root Password for XenServer Host</a></li>
	<li><a href="https://xenserver.org/blog/entry/resetting-lost-root-password-in-xenserver-7-0.html" target="_blank" rel="noopener">Resetting Lost Root Password in XenServer 7.0</a></li>
	<li><a href="http://www.sanitarium.co.za/how-to-reset-the-root-password-in-xenserver-versions-5-0-and-later/" target="_blank" rel="noopener">How To reset the root password in XenServer versions 5.0 and later</a></li>
	<li><a href="https://linuxconfig.org/how-to-reset-an-administrative-root-password-on-xenserver-7-linux" target="_blank" rel="noopener">How to reset an administrative root password on XenServer 7</a></li>
</ul>
You ask yourself, "<em>Self, where are the instructions on changing the root password when you already know it?</em>"

A quick look at the XenServer <a href="http://docs.citrix.com/content/dam/docs/en-us/xenserver/7-1/downloads/xenserver-7-1-installation-guide.pdf" target="_blank" rel="noopener">install</a> guide and <a href="http://docs.citrix.com/content/dam/docs/en-us/xenserver/7-1/downloads/xenserver-7-1-administrators-guide.pdf" target="_blank" rel="noopener">admin</a> guides don't reveal anything either.

# Yes, you can! #
Citrix support wasn't much help in this, but the answer is quick and easy, especially if you have XenServer pools.

First connect to your XenServer (use the Pool Master if you have a pool), and get to the console.
![xenserver11](/assets/img/xenserver-change-root-password/xenserver11.png)

Select **Authentication**
![xenserver2](/assets/img/xenserver-change-root-password/xenserver2.png)
![xenserver3](/assets/img/xenserver-change-root-password/xenserver3.png)

Select **Change Password**
![xenserver4](/assets/img/xenserver-change-root-password/xenserver4.png)

Authenticate with your current password (if prompted).
![xenserver](/assets/img/xenserver-change-root-password/xenserver.png)

Enter the old password, followed by the new password twice.
![xenserver7](/assets/img/xenserver-change-root-password/xenserver7.png)

Once you hit enter, the system will change the password.
![xenserver8](/assets/img/xenserver-change-root-password/xenserver8.png)

And you're done.
![xenserver9](/assets/img/xenserver-change-root-password/xenserver9.png)

###### BONUS: If you changed the password on the Pool Master, this will change the root password on all the pool member servers. ######

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*