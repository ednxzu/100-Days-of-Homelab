# 100 Days of Homelab: Day #4 - A lot of stuff
I figured it out !!!

## NFS Debug 
The hardlinks were what was causing NFS stales ! If you have them, and are exporting NFS shares, that's probbly what is causing NFS stales. Turning hardlink support off fixed all of my NFS issues (which makes sense considering how NFS references files).

## Portainer templates
Otherwise, today was not super interesting, since I just worked on a json file to add all of my yaml stack files as templates for my portainer instance ([See docs here](https://docs.portainer.io/v/ce-2.11/advanced/app-templates/format)). I would like to have most of the containers usable by other people (friends, etc...) on my platform, without having to go through the pain of figuring out which settings work or not... So portainer templates are looking very good for that use-case,we'll see how it goes !


See you tomorrow !