layout: page
title: SHH between two remote servers where one requires a VPN
---

Recently, I found myself in a situation where I needed to copy a large amount of data between two servers I was working on.
Both servers allowed login via my public keys, stored on the respective server, but no password login.
Additionally, one of the two servers is not publicly accessible, but requires accessing the network it is connected to using a VPN.

Copying the data to my local machine, and subsequently transferring it to the target server was out of the question since the data were too large, and the bandwidth too slow.

After a long time spent searching, the solution to my problem turned out to be quite simple: if the user's keys are managed by the ssh key-agent, ssh allows the user to "take the keys with them" when establishing an ssh connection with a target. Once logged into the first server (using the VPN connection), I was then able to authenticate to the second server and copy the files directly.

Here is how I solved the file copy riddle, in hopes that it might save someone some research:

0. make sure all your ssh keys are added to the key agent on your local machine (using `ssh-add $your_key_file`)
1. establish a connection to the first server, using ssh with the -A option: `ssh -A $user1@server1.xyz`
2. copy files using e.g. scp: `scp $user2@server2.xyz:yourfile target/locaton/on/first/server`


