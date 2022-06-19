# 100 Days of Homelab: Day #8 - Portainer, Hardware, and BunkerWeb
Hum... today was not so sexy... Except for one thing !

## Portainer templates
I spent most of my time writing portainer template files for my docke swarm cluster. This was a long, repetitive process that frankly bored me to death... but it had to be done so that I can deploy things from the UI while writing is to my gitlab project.

## Hardware verifications
I also spent some time reviewing some other hardware I received for my homelab upgrade.

## BunkerWeb
And I found this awesome tool called [BunkerWeb](https://docs.bunkerweb.io/1.4/#security-features) which seems to be more or less what I want for my public reverse proxy. In shot, it's a security-upgraded nginx reverse-proxy that allow you to configure captcha, anti-bot solutions, blacklists, etc... for nginx either via CLI or with their WebUI. I might try it out in the coming weeks, and I might even end up replacing my current NPM reverse-proxy for the public side (internal reverse-proxy will be handled by traefik).

See you tomorrow !