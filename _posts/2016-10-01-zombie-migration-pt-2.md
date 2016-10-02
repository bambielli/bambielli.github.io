---
title:  "Part 2: Zombie Migration"
date:   2016-10-01 21:31:00
category: post
tags: [beer, nginx, raspberry-pi, django]
---

I continued to set up my RBIP as a web server this weekend: I've finally got it publically facing on a static IP, and the [Zombie Dust][zd]{:target="_blank"} checking script is running every 10 minutes. Here are some thoughts from the experience

### N band wireless = Slow Dependency Downloads

I'm using a little USB wireless adapter that I had lying around to connect my RBPI to the internet. Unfortunately the adapter is a few years old, and only supports N band wifi... The wifi in my apartment is pretty damn fast, but I can only connect to my 2.4GHz band, so downloading and updating dependencies on the box were slow going.

Just thinking out loud: I guess the only way the speed of my internet would become a performance cocern for the app is if I needed to perform a bulk download / upload of data before completing an action or rendering a page. Luckilly the app is not very network intensive: the most intensive part would be sending out emails to users who have subscribed to the notification service, but since the emails aren't time sensitive this doens't seem like it will be an issue.

### Setting up Debian Services - Daemons

I learned how to configure a few [debian services (AKA Daemons)][daemon]{:target="_blank"} as part of this project. These are essentially background processes that will run until they are told to stop. Services elligible to be run as a Daemon are listed in the directory `/etc/init.d/` and can be invoked by running `sudo service service-name start`. The three services that I configured to run as Daemons are 1) postgresql server 2) redis server 3) nginx.

If I needed to reboot my RBPI for any reason, I wanted to minimize the number of commands I'd need to remember to run to get my application back up and running. I configured the three services I mentioned above to run on startup by running `sudo update-rc.d service-name enable`. I hadn't worked with Daemons before (but had always seen them running in task manager) so now I have a better understanding of what they are, how they work, and where they are configured.

### Security

Since I would be exposing my server to the public internet, I wanted to make sure I implemented some security measures first. I won't list all the steps I took here, but an essential step is to set up a firewall to whitelist ports on which communication can occur. Important ports to whitelist for web servers are port 80 and 443, for http and https communication respectively.

I did some additional configuration to ensure that users trying to ssh (port 22) on to the device would only be able to do so via public key encryption... if they don't have their public key on the server, they won't be able to ssh on! I also locked down ssh for only particular users on the server, who have limited access (non root access) to the device.

### Whats next?

The app is up and running, and exposed to the interwebs, but I haven't swapped the domain over from the one pointing at heroku (doesfloydshavezombie.com). I'd like to leave the app running for a bit longer before switching the production domain over. I also have about 100 users signed up in production currently, so I'll need to do a migration of those addresses over to my RBPI before making the domain swap. I'll need to put some thought in to the migration plan, to ensure service is not disrupted during this transition period.

I'm also noticing that notification emails being sent from my app are going straight to my spam folder in gmail, which didn't used to occur. I can't imagine that swapping the hosted location of my application would cause the emails to suddenly be sent to spam (they are ultimately being sent from the same email address as they were before). The emails themselves are not very rich... just a piece of text with the notification and an unsubscribe link. I might spend some time spicing these up a bit, but I know getting HTML5 emails to look good on all device types (mobile, desktop, different clients) can take a lot of doing.

I definitely see the value in a service like Heroku as well: the ability to just push up  an application with a list of dependencies and have the heroku deploy process take care of the rest is pretty spectacular. Offloading the setup and maintenance of your application infrastructure to a PaaS saves a ton of time, and gets developers back working on the application itself.

[zd]: https://www.3floyds.com/beer/zombie-dust/
[daemon]: https://wiki.debian.org/Daemon
