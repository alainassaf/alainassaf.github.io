---
layout: post
title: Resetting nsroot password on NetScaler MAS
date: 2017-09-14
readtime: true
tags: [Administration, Netscaler MAS, Reporting, Monitoring]
thumbnail-img: /assets/img/netscaler-mas-resetting-nsroot-password/1vst4d.jpg
share-img: /assets/img/netscaler-mas-resetting-nsroot-password/1vst4d.jpg
---
![mrrobot](/assets/img/netscaler-mas-resetting-nsroot-password/1vst4d.jpg)

# Intro #

NetScaler MAS represents a very versatile and powerful tool. If you have NetScalers, I recommend you give it a try. As of this writing, if you have 30 or fewer VIP's configured on your NetScalers, you can use all the features of MAS (confirm with your Citrix Sales Rep). So, let's say you want to make some changes to MAS and find that a former employee has removed write access to your group and the nsroot password is unknown. What do you do? Citrix provides documentation on resetting the nsroot password on NetScalers, but nothing on MAS.

## Get your Mr. Robot on! ##

We were able to follow most of the procedure in [https://support.citrix.com/article/CTX232550/how-to-reset-nsroot-password-on-netscaler-mas](https://support.citrix.com/article/CTX232550/how-to-reset-nsroot-password-on-netscaler-mas)

This was a virtual machine on XenServer, so I connected to the console
<ol id="task_0FB7BEDEBF1342E784A473609A655695__steps_E895BA18FBE14D45835F25C6269DA707" class="ol steps">
	<li id="task_0FB7BEDEBF1342E784A473609A655695__step_D817E9AFFEAB4D0A86D06A334089380F" class="li step"><span class="ph cmd">Connect to the virtual appliance via XenCenter..</span></li>
	<li class="li step">Reboot the NetScaler MAS.</li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__step_EC7D95EF3703454EBDCB633F9601DF53" class="li step"><span class="ph cmd">Press <span class="ph uicontrol">CTRL+C</span> when the following message appears:</span>
<pre class="p">Press [Ctrl-C] for command prompt, or any other key to boot immediately.

Booting [kernel] in # seconds.</pre>
</li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__step_13713B9DB65B4743B486A7EAB605C48B" class="li step"><span class="ph cmd">Run the following command to start the NetScaler in a single user mode:</span>
<pre class="p"><span class="ph uicontrol">boot -s</span></pre>
<div class="p">
<div class="note note"><span class="notetitle">Note:</span> If <span class="ph uicontrol">boot -s</span> does not work, then try <span class="ph uicontrol">reboot -- -s</span> and appliance will reboot in single user mode.</div>
</div>
<p id="task_0FB7BEDEBF1342E784A473609A655695__p_2BC1A31CDD184588AC5562966CDF0202" class="p">After the appliance boots, it displays the following message:</p>

<pre id="task_0FB7BEDEBF1342E784A473609A655695__p_3302318F383149FDB4134C95EA07606F" class="p">Enter full path name of shell or RETURN for /bin/sh:</pre>
</li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__step_2C4C97BD32A44D919875BEA2041C55F0" class="li step"><span class="ph cmd">Press ENTER key to display the prompt, and type the following commands to mount the file systems:</span>
<ol id="task_0FB7BEDEBF1342E784A473609A655695__substeps_15CAD745FBCF4FC79BF6DBA3A90137AE" class="ol substeps">
	<li id="task_0FB7BEDEBF1342E784A473609A655695__substep_4CFE3AD1D9734D80A49FCD098BC29D57" class="li substep"><span class="ph cmd">Run the following command to check the disk consistency:</span>
<pre class="p"><span class="ph uicontrol">/sbin/fsck /dev/ad0s1a</span></pre>
<div class="p">
<div class="note note"><span class="notetitle">Note:</span> Your flash drive will have a specific device name depending on your NetScaler; hence, you have to replace <span class="ph uicontrol">ad0s1a</span> in the preceding command with the appropriate device name. In my case it was <span class="ph uicontrol">ad0s1a</span></div>
</div>
<div></div></li>
	<li>If you receive the following after running the above command:
<pre>fsck: Could not determine filesystem type</pre>
</li>
	<li>Run this command to resolve:
<pre>/sbin/fsck_ufs /dev/ad0s1a</pre>
You should see the following (select Y for all the prompts).
<img class="alignnone size-full wp-image-3519" src="/assets/img/netscaler-mas-resetting-nsroot-password/nsroot1.png" alt="nsroot1" width="721" height="478" /></li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__substep_25BFEA9456AF43258900F8936A2951BB" class="li substep"><span class="ph cmd">Run the following command to display the mounted partitions:</span>
<pre class="p"><span class="ph uicontrol">df</span></pre>
<p class="p">If the flash partition is not listed, you need to mount it manually.</p>
</li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__substep_58BDBC0520DC4086B8D4A12C211B2950" class="li substep"><span class="ph cmd">Run the following command to mount the flash drive:</span>
<pre class="p"><span class="ph uicontrol">mount /dev/ad0s1a /flash</span></pre>
</li>
</ol>
</li>
	<li id="task_0FB7BEDEBF1342E784A473609A655695__step_CB656213BCE04C33AAE404E295051BF5" class="li substep"><span class="ph cmd">Run the following command to change to the nsconfig directory:</span>
<pre class="p"><span class="ph uicontrol">cd /flash/mpsconfig</span></pre>
</li>
	<li>Create a hidden recover file in this directory
<pre>touch /flash/mpsconfig/.recover</pre>
</li>
	<li>Reboot the MAS</li>
	<li>Once the reboot completes, enter in nsroot/nsroot for the username and password (this may take a couple of minutes before you can login with nsroot.</li>
	<li>Login to the MAS web page with nsroot.</li>
	<li>Change the nsroot password and make other administrative changes.</li>
</ol>

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*