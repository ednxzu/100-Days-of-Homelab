# 100 Days of Homelab: Day #4 - A lot of stuff
Today was spent mostly on debugging NFS stale file handles... 

## NFS debug
I tried a couple of solution that I found here and there, but overall, no real progress on why this is happening (in details I mean).the _netdev mount option seemed to do good, as well as setting the fuse_rememeber option to -1 on the server side.. Both ended up still giving me global stales (on all hosts at the same time... so it has to be a server issue right ?). tried the local-lock=none thing aswell but that was a dead end aswell.. One very promising lead seems to be the hardlink support in unraid. Apparently, the way unraid uses its cache array is by hardlinking stuff from the storage array, to the cache one, which seems to freak out NFS (of fuse?).



Tomorrow I'll dig some more on that, since I really need NFS shares for my docker volumes...
See you tomorrow !