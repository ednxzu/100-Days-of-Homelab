# 100 Days of Homelab: Day #4 - A lot of stuff
So... unlike yesterday, today was rather productive !

## NFS Shares config on my unraid server 
I took some time to configure mon shares in unraid the way I want them to be in the end. This included moving everything I had in datastore2 (a lot of junk/old dotfiles/unfinished apps and other projects) to a new datastore4, that will be here for the sole purpose of handling temporary stuff.
then I made datastore3 and emptied datastore2. My current production datastore is datastore1, which I can't really touch or people will be raiding my house asking why plex is down...
So I just built the future architecture of datastore1 next to the existing one (which is horrible, it involves spaces in directory name, you don't wanna know about it...)

The newdirectory tree looks like that (yaml formatted):

```yaml
unraid_server:
  datastore1:
    - media:
        movies:
        tv:
        music:
    - torrents:
        movies:
        tv:
        music:
    - usenet:
        movies:
        tv:
        music:
  datastore2:
    - swarm:
        nextcloud:
        wiki:
        gitlab:
    - backups:
  datastore3:
    - lanson:
  datastore4:
```
Next, I configured the NFS exports for the shares, using the integrated NFS tools from unraid
I exported everything as private shares, adding the following config to each share that had to be exported
```
(sec=sys,rw,insecure,all_squash,anongid=1000,anonuid=1000)
```
**NOTE**: This will have to be tweaked depending on my needs, and I'll monitor the mounts to see if i get the infamous NFS stales. 

## Ansible automation
Next, I wrote some ansible code to add to my already running ansible automation (I deploy it with Gitlab CICD pipelines by creating an ansible container, It's really cool, I can't wait to show you!). This code mounts the NFS shares on each host where it's needed (mainly the docker swarm hosts for now).

Next step here will be to polish this code in order to make it as close to what I need as possible.

## The quest of finding the right LDAP server
In my quest for finding a good authentication database, I found [LLDAP](https://github.com/nitnelave/lldap), which honestly was a relief. It's dead simple, integrates with pretty much anything I want, runs in docker, has an integrated UI, the whole 9 yards! I managed to get it to work with authelia in like, 5 minutes ? (2 of which were just me finding typos in my config file). I 'll check later this week if it can be used by sssd to handle account on my servers, but even if it doesnt, I'll probably keep because of how low-maintenance it seems to be.

## Hardware purchases
I also bought some of the last stuff I'll need for the final rack, including shelves for my optiplex micro computers, a rackmount kit for the edgerouter, patch cables, patch panels, keystone rj45 double-sided, and some other stuff I probably forgot about.





Overall, a very productive day, and I feel very good about this LLDAP thingy ! try it out, it's awesome !
See you tomorrow !