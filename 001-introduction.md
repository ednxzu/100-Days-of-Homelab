# 100 Days of Homelab: Day #1 - Introduction
Recently,  the tech youtuber [TechnoTim](https://www.youtube.com/c/TechnoTimLive) reached 100k subs on Youtube, and decided, with several other tech youtubers, to put a fun challenge, the 100 days of homelab !
The rules are simple, dedicate at least 1 hour a day for the next 100 days straight, to you homelab, for building, planning, learning, etc.., and share you results with the #100DaysOfHomeLab.


## The Plan
So let's do it ! I will use this challenge to (finally) move from my old homelab that has not really changed for the last 3-4 years, to my new infrastructure, and I'll document all of it here !
This will be done in not particular order, and I will try to provide as much technical explaination as possible, so that you might copy and redo some parts in your own environment.
Hopefully, by the end of this challenge, I will also share with you my private wiki, as my contribution to this awesome community. But enough talking, let's get started !

## What's to build
Starting with the **network**, the new infrastructure will have will have a rather simple scheme, with:

**1. Ubiquiti Edgerouter 4**

    This will act as the router and DHCP server of the entire network.
    It will define the VLANs and distribute DHCP inside of them.
    It will also handle the firewall.

**2. D-Link DGS 1210-16**

    This switch will be the main switch, to which will connect the servers, 
    laptop, and everything that needs a wired connection but doesn't require PoE (power over ethernet). 
    The great thing about this switch is that it is fanless, and has 4 combo ports (rj45/SFP) 
    that I can use for the uplink to the router and the second switch.

**3. D-Link DGS 1210-10P**

    This second switch, which is also a fanless one, will handle the PoE devices, 
    such as the access points for my wireless network, cameras, etc..
    It has 2 SFP ports which I will be using to connect to connect to the main switch.

**4. Unifi UAP-AC-Lite**

    I have a couple of them, and they will be used to handle everything and anything that needs a wireless connection, such as cameras, phones, laptops, etc..
    I have been using them for a couple of years now and they fit my needs perfectly so I'll keep using them.

Annnnnd, that's about it for the network side. I don't really want to over-complicate things on the network, since I won't be doing super complex things on it, and I would rather focus on the computing infrastructure side of things to do my weird stuff.

---

Talking about the computing part of things, here's the plan:

**1. Unraid Server**

This will be my main storage solution, with 28TB of usable storage made out of an unraid array, and 500GB of NVMe cache in RAID1. This machine will also host a couple of VMs, such as my secondary dns server, my home assistant server, and a reverse proxy VM for the outside traffic (more on that in a bit). I find unraid extremely fitted for the job, since it is stable as a rock, relatively easy to use, yet offers a lot of options for virtualization and containerization. Speaking of containerization, this server will also host my jellyfin server, since it has a quadro P620 GPU in it, that I'll be using for transcoding media to my clients.

**2. Proxmox Server**

Another rock solid OS that fits perfectly into a homelab is Proxmox VE. I plan on setting a small, 6cores/16GB server that will host a couple more VMs, that wont require a lot of performance, but need to be super stable. This include my freeipa(ldap) server, my main dns server, and a couple of test VMs for my ansible CI (more on that also in a bit). From my experience, Proxmox has been my most stable server OS in the past 3-4 years, and that's why I intend to put my most critical stuff on it. For now I don't plan on making this a higly available cluster, but I might get there if I encounter outages.

**3. Docker Swarm Cluster**

This is where the fun really begins ! I recently acquired 3 optiplex 3080 micro from DELL, and loaded them with 24GB of DDR4 RAM each. I will be setting up a Docker Swarm cluster to host all of my services in containers. This way I can optimize my ressources and deploy most of my services in a highly available environment, while also allowing people (friends mostly) to use this cluster for their own needs, just like they would with any public cloud platform.
The High Availability of the cluster will be handled by a keepalived virtual IP address, but we'll talk more about that in a future post.

**4. CICD and Software Defined Infrastructure**

All of this seems like quite a mess at first glance, and it would be, if it wasn't managed and configured properly ! I will be using Gitlab CI platform abd absible to keepp all of my infrastructure under control, with scheduled and commit-based jobs that will verify, test, and deploy my changes to the servers in an automated way (you'll see, it's super cool !). This ease the management of all the servers quite a bit, and standardize everything so that debugging isn't too painful.

And voila ! In a 100 days, so on September 20th 2022, I should have all of this setup and ready to go live.
And of course, I will document every step of the process so that hopefully it can help someone.
That is my small contribution to this amazing community that is the thech/homelab/selfhosting community.

See you tomorrow !

![diagram](https://github.com/ednxzu/100-Days-of-Homelab/blob/main/ressources/homelab.jpg?raw=true)