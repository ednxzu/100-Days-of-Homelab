# 100 Days of Homelab: Day #2 - FreeIPA first steps
So... today was not super productive, as I've had a few personal setbacks, but here I am.

## First try at  FreeIPA for centralizing my authentication
My first idea for centralizing my authentication was to use a discord server for both me and all my friends/family to join, and use [authentik](https://goauthentik.io/) to handle SSO via discord logins, so that people wouldn't need yet another account for my services. But since Discord is now partially owned by Tencent, and that their practises with user datas have not been quite good for the past few years, I settled for a self hosted authentication service.

[FreeIPA](https://www.freeipa.org/page/Main_Page) seems like the better option from what I've seen online, as it includes pretty much everything you might need into a single server, and it has an integrated webUI (that's a battle won against [openLDAP](https://www.openldap.org/))

One problem I had with it was that it doesn't natively come bundled for Ubuntu. As I wanted to normalize my servers, I settled for Ubuntu as my main OS for them, and unfortunately that wouldn't be easily possible for FreeIPA. 

So, for the test deployment (this will not be the final server), I installed a server based on [Fedora](https://getfedora.org/) 36, and followed more or less a tutorial made by the amazing people over at [Ibracorp](https://ibracorp.io/) (Seriously, go check them out, they're awesome).

I had in running in no time, but for some reason (it may have been my VMs that was too small), It felt like the dnf package manager was super slow (compared to pacman/yay, or even apt), so it took quite a while to install everything and update the system (around 10 minutes, nothing unbearable, but still..).

One cool thing tho is that fedora comes with [Cockpit](https://cockpit-project.org/) by default, which is a nice feature to have, even tho i'm not sure about the ressource usage just to run it.

## Impressions
FreeIPA seems like a very viable solution for handling users inside of a homelab in my opinion. It's fairly simple to install, got an integrated webUI instead of a client one (like apache directory studio), and it comes with possibly everything you would need for a homelab. 

I'll probably dig a little more into it in the coming days, now that the setup part is behind me, and see how far I can take it.

The only downside in my opinion is that it doesn't come packaged for Ubuntu, which I know a lot of people use as server OS for their lab, or even at their company. I'll probably try to either recompile it to work on Ubuntu, or try to use their dockerized installation, so that I don't have a single Fedora server in my all-Ubuntu environment, but that's for another day..

See you tomorrow !