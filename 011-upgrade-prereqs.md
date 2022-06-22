# 100 Days of Homelab: Day #11 - Preparing the upgrade
I should have received all my new hardware by the end of the week ! It's time to prepare the upgrade !

## Cleaning up
Today was spent perfecting stuff I had already done, in order to prepare for the upgrade, that should happen on sunday. I starte emptying the cache array from my unraid server, and disabled it for all my shares so that it doesn't fill back up. I also re-wrote my stack files for my docker swarm cluster.

## Planning the rest
I made a list of the things that are still to do before the upgrade, so that I don't forget anything hopefully.

- backup test swarm data
- reinstall swarm-nodes with ubuntu 22.04
- prepare ubuntu 22.04 templates for proxmox
- replace current test-vms with ubuntu 22.04 ones
- deploy pihole vm proxmox
- deploy pihole vm unraid
- setup gravity sync
- save current unraid array config
- save current switches configs (DGS 1210-16 and 1210-10P)

If I can do all of that before sunday, it'll be great, and the upgrade should be pretty straight forward (NOT ! there's DNS involved, it's never straight forward...)

See you tomorrow !