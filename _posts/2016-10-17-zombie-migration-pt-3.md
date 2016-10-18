---
title:  "Part 3: Zombie Migration - It's ALIVEEEE"
date:   2016-10-17 20:50:00
category: post
tags: [beer, nginx, raspberry-pi, django, security]
---

Wow. There was a lot more work to do than I expected to get [doesfloydshavezombie.com][zombie]{:target="_blank"} back up and running. Here's the final entry in this series!

### [Setting up SSH][ssh]{:target="_blank"}

I hadn't set up publicKey authentication on my own before, so it took me a while to figure out where to create the keys and which keys to transfer where...

The learning here was: you want to create your key pair on the client with which you are expecting to access the server, then transfer the generated public key over to the server (using something like ssh-copy-id) under a particular user's ssh key list.

### A Not-So-Static IP

While attempting to transfer my ssh key over to my RBPI, I realized the IP address of my device had changed... previously I had modified /etc/network/interfaces and changed wlan0 config from dhcp to static, then assigned my IP address there. Apparently this wasn't the best, or easiest solution.

Instead, I modified my server's /etc/dhcpcd.conf file, [specifying a new IP for the wlan0 interface][static]{:target="_blank"}. After a reboot, I was officially locked on a static address.

### A Not-So-Public IP

I had given my device a static IP address, but the assigned IP was only in relation to my router's internal address space (192.168.1.xxx).

In order to expose my device to the outside world, I first needed to give my router a static IP instead of one dynamically assigned my ISP. This is easy to do in the router's settings.

My RBPI was already running an nginx webserver that was listening for requests on http port 80. nginx routes requests on port 80 to my django application, which runs in a uwsgi server. To be able to access my application from this static IP, I needed to set up Port Forwarding for requests to port 80 to my RBPI's static IP in the router's address space. I set up Port Forwarding for SSH as well while I was here (port 22) and after re-configuring the firewall I had previously set up on the server, I was able to successfully ssh on to the server and make requests to the application via port 80.

### uwsgi Reconfig

Previously I was booting up my uwsgi server, explicitly binding it to port 8001 on startup. Apparently there is a better, more direct way of getting nginx and uwsgi to communicate with each other, which is via unix sockets using a `.sock` file.

 > File sockets are ways for processes on the same host to talk with each other. This communication happens via the `.sock` file.

It took me a while to understand what I was doing when setting up the `.sock` file. This isn't a file that you create explicitly: by specifying in the nginx config that my application could be contacted via a `.sock` at a specific path, the file would be created when the uwsgi container was run. Even better, if it is created in some root level directory in the filesystem (like /tmp/socket/myapp.sock) then you don't need to worry about making sure permissions on the file are configured appropriately.

I also created a `.ini` file for my uwsgi container, that contained all of the configuration I needed for my app container, including socket information. I installed uwsgi globally (making the mistake initially of installing using global pip, which was for python 2.7 when I needed 3.4...) then added a line to my `/etc/rc.local` file to make the uwsgi server run in the background when the server boots up, writing logs to /var/log/zombie-logs.log

Finally, I changed my raspi-config to have the RBPI boot to a terminal instead of to the desktop, since I would no longer need to access the desktop to install or test anything further. I could also do some cleanup of some of the bloatware included with the jessie raspbian image (like minecraft PI and Wolfram Alpha) but maybe for another day...

### Last But not Least

Now that the application was running smoothly and server was secured, the only thing left to do was to point the domain name for [doesfloydshavezombie.com][zombie]{:target="_blank"} from the Heroku app to the RBPI instead. I deleted the CNAME records I had created to point at the heroku app, and instead created an A Record to point at my router's static IP. The transfer took about 5 minutes to complete (namecheap is pretty fast).

Another nice part of having a domain name pointing at the static IP for the router, is that now I could SSH on to the app with the command `ssh user_name_here@doesfloydshavezombie.com` instead of needing to remember what the actual IP numbers were. Awesome!

I lied... one final step: in order to achieve the goal of officially deleting the heroku app instance I had provisioned for the site, I also needed to do a data transfer from my heroku postgres instance to the one on the new server. After installing the heroku CLI + toolbelt on the server, I was able to curl down a dump of the database and pg_restore it to the database on my server. I did a quick user count after I did the upload: **103 users were still signed up to receive my notifications** even though the site had been down for the past few months. That feels good :)

### It's Aliiiiivvveeeeeee

When I started this project a few months ago, I had no idea the amount of configuration that would be required to get everything working. Having used heroku for the majority of my infrastructure needs over the years, a lot has been abstracted away. For small projects and small startup teams, a service like heroku is very valuable so developers can get working on things of value instead of twiddling away with securing and optimizing servers.

That's not to say that this experience hasn't been valuable for me. Setting up a webserver from scratch pulled away layers of abstraction that I was used to working with, and forced me to re-learn concepts that I had previously forgotten (like port forwarding, and socket communication). It's quite satisfying to get everything up and running from scratch.

A successful project! Let the Zombie Dust flow!

[zombie]: doesfloydshavezombie.com
[ssh]: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2
[static]: https://www.modmypi.com/blog/how-to-give-your-raspberry-pi-a-static-ip-address-update


