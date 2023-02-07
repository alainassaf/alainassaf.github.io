---
layout: post
title: Fish and dead hosts stink after three days
subtitle: XenServer
date: 2011-02-10
tags: [Administration, Citrix, XenServer]
cover-img: ["/assets/img/xenserver-fish-and-dead-hosts-stink-after-three-days/deadfish.jpg" : "Photo by S Turby on UnsplashS"]
thumbnail-img: /assets/img/xenserver-fish-and-dead-hosts-stink-after-three-days/deadfish.jpg
share-img: /assets/img/xenserver-fish-and-dead-hosts-stink-after-three-days/deadfish.jpg
---
<strong>Prolog:</strong>
We had a XenServer go down and it required a rebuild.  The problem was that we could not use the same name until the old server was removed from XenCenter.  Using `XE Host-Forget UUID=<Host UUID>` did not work because the pool master thought a VM was still running on the missing server.  However, using some other, more drastic commands, we managed to remove the host UUID from the pool master so we could not use the typical XE commands to shut down the VM and remove the host.

<strong>Get to work:</strong>

Here are the steps I used to remove the host (<em>these commands were culled from different sources, so hopefully putting them in once place will be a benefit)</em>.  <em>For the purposes of this example the dead server will be known as FISHHEAD and the pool master IP will be 192.168.1.1.</em>

I first had to get the UUID of FISHHEAD which was no longer in the host list, so I ran this command from a server with XenCenter installed:

```
xe –s 192.168.1.1 – u root –pw <MYROOTPW> pool-sync-database
```

This generated the following error:

```
You attempted an operation which involves a host which could not be contacted.
host: 5491fe8d-70ae-4a82-aae1-ab2719f1469e (FISHHEAD)
```

Now with the UUID of FISHHEAD I ran this from the pool-master’s console.

```
xe vm-list resident-on=5491fe8d-70ae-4a82-aae1-ab2719f1469e
```

Which listed FISHHEADGUEST (UUID=1d75984e-1b9c-0ea0-0a22-8db2175ca70f) which was preventing the removal of FISHHEAD.

I used this command to power off FISHHEADGUEST :

```
xe vm-reset-powersate uuid=1d75984e-1b9c-0ea0-0a22-8db2175ca70f force=true
```

Finally, I could use host-forget to remove FISHHEAD.

```
xe host-forget UUID=5491fe8d-70ae-4a82-aae1-ab2719f1469e
```

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*