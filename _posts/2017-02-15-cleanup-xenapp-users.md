---
layout: post
title: Cleanup XenApp Users
date: 2017-02-15
readtime: true
tags: [Administration, PowerShell, Scripting, XenApp]
thumbnail-img: /assets/img/cleanup-xenapp-users/dino.jpg
share-img: /assets/img/cleanup-xenapp-users/dino.jpg
---
<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/cleanup-xenapp-users/dino.jpg" 
    alt="dino">

# Intro #
One of the signs of a successful IT team is to have clearly defined environments for development, testing, and production. To actually put this into practice however, requires enough head count and enough resources (real and virtual) to maintain it. Many Citrix shops don't have these luxuries. In this post, we will look at a script I wrote to help cleanup the users/groups assigned to Published Applications in a XenApp 6.5 farm that was used for development, testing, and production.

# Hey, we need to test an app... #

This phrase is met with trepidation by many Citrix Admins. Your co-worker has just sent an email with scant details and many assurances that this application test is just a POC and won't have many users or require many resources. So you (being awesome) get the application installed and working and the POC starts.

First, you add one or two users, then more and more, then a group that holds most of the department that wanted to test this application to being with. Within a month, you're told the POC was wildly successful and they already purchased the software. Now you have an application with a "Configured Users" section that is out of control

| ![cleanup1.png](/assets/img/cleanup-xenapp-users/cleanup1.png) |
|:--:|
| Yes, this is a real production application |

So, if you have a number of applications like this how can you clean things up or at least get some feedback on what should change?

# Putting It Together #
### Loops and loops ###
{% gist 22c433756a49e1bfa669d998e14ba2d9 %}

We save all the active applications to a variable and just keep the application name and assigned accounts. Then we start looping through the accounts for each application. If the account matches the regular expression (see the <strong>$accountpattern</strong> variable in the full script below) for user accounts, then it is added to the <strong>$isUser</strong> variable. Otherwise, we add the AD Group to the <strong>$isGroup</strong> variable (skipping the "Citrix Admins" group as it is already added to every application).

### Act on the information ###
{% gist 326c7fb8d167411eca1cca31af549688  %}

Keep in mind that we are still in the second account foreach loop. Review the <strong>$isUser</strong> and <strong>$isGroup</strong> variables. If a user is a member of one of the groups already assigned to the application, make a note in a text file to remove the user from the application. If a user is not a member of any groups, then recommend that a new AD group be created for this application and add those users to it. Otherwise, if there are no groups associated with the application, recommend that one get created. You can step through the if..else logic with some test applications and add actual active directory actions to further automate the script.

At the end you will end up with a text file (two if you use the <strong>$addADGroupList</strong> switch variable). The xaappfixes_datetimestamp.txt file has a list of users to remove from groups and suggested AD (Active Directory) groups that should be created. The xaappgroups_datetimestamp.txt just lists suggested AD groups that you could run through a AD script or hand over to your AD team.

Here is an example of the xaappfixes file

<img 
    style="display: block;
		   margin-left: auto;
           margin-right: auto;"
    src="/assets/img/cleanup-xenapp-users/cleanup23.png" 
    alt="cleanup23">

Here is an example of the xaappgroups file

<img 
    style="margin-left: auto;
           margin-right: auto;"
    src="/assets/img/cleanup-xenapp-users/cleanup3.png" 
    alt="cleanup3">

# The Script #
You can get the script from <a href="https://github.com/alainassaf/cleanup-xaappsandaccts" target="_blank">GitHub</a>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*