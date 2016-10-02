---
title:  "Part 1: Zombie Migration"
date:   2016-09-25 15:53:00
category: post
tags: [beer, nginx, raspberry-pi, django]
---

This weekend I decided to dust off the RaspberryPi I've had lying around and get it up and running as a web server again. I'm currently hosting my zombie-dust checking application on heroku: since the app isn't doing anything strenuous, I figured this would be a good candidate to run off of the RBPI web server. Definitely better than this blog, since I want the blog to be as speedy as possible, while the main function of the zombie app is to initiate a cron job every minute to scrape HTML off of the 3Floyds beers to go page... performance doesn't really provide any benefit.

### Server - The Raspberry PI

The RaspberryPi I have is a few years old and had an image of raspbian on its microSD card. Unfortunately, when I booted it up, the image appeared to be corrupted. I downloaded a fresh copy of raspbian, re-formatted the SD card as FAT-32, and [used diskutil from the command line to unmount the SD card and copy the fresh raspbian image on to it][raspbian]{:target="_blank"}. This worked like a charm!

Secondly, I needed to freeze the IP address of my RBPI to a static address, so I can map a domain name to it in future steps. I accomplished this by following [step 2 from this guide][static]{:target="_blank"}. I know there is a way to freeze the IP of a device in the settings of a router, but I hadn't done it this way before so it was a good learning experience.

Next I needed to install a webserver on the raspberry pi, as well as a uWSGI application interface so the webserver would be able to communicate with the Django application (zombie-app is written as a Django app). I [cloned the app from github][zombie]{:target="_blank"}, created a virtualenv wrapper for the application and installed necessary dependencies for the project using pip [(including installing the latest version of postgresql manually)][postgresql]{:target="_blank"}. Then, I installed uWSGI and nginx.

### Testing

Before going live in production, I want to make sure emails will still be appropriately sent from the application. I'll leave the app running from the raspberry pi for a few days, to make sure all things look good before the general release.

### What I learned

Installing python packages on a RBPI takes forever, and is dull as hell. Trying to install uWSGI took about 30 minutes. I guess this is partially because the wifi adaptor I have can only detect 2.4 ghz wifi, and my RBPI is pretty far away from the router, but even compiling and linking the source for the uWSGI library took forever. This little processor is slooooowwww.

Also I forgot that my application was written using python3, so I installed everything in a python2 virtualenv and then the app wouldn't boot up...

Also I forgot that heroku takes care of a few other infrastructure type things for you, like setting up a redis server and postgresql server... Before I can get this app up and running I'm going to have to install both and serve everything from the raspberry pi. This is gonna be a lot harder than I thought.

To be continued.

[raspbian]: https://www.raspberrypi.org/documentation/installation/installing-images/mac.md
[static]: http://projpi.com/diy-home-projects-with-a-raspberry-pi/pi-web-server/
[zombie]: https://github.com/bambielli/zombie/tree/59e534ccf2b1bd64691fe6d923c3ff04554f7e39
[postgresql]: http://raspberrypg.org/2015/06/step-5-update-installing-postgresql-on-my-raspberry-pi-1-and-2/

