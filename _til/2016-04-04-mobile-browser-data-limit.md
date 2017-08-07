---
title:  "Mobile Browser Data Limit"
date:   2016-04-04 20:48:00
category: til
tags: [mobile]
---

TIL that there exists a hard cap on the amount of data a single webpage can ship to a mobile browser, before the browser crashes.

## Imagine an API endpoint...

This endpoint ships the names and group affiliations of a particular survey population. It assembles this list of users on the backend, constructs a JSON object with symantically named keys, zips and caches the object in the application cache (to service future requests) and ships the zipped response to the requestor.

Users log in to the survey on a laptop, and are greeted with a list of selectable users for the survey they are taking, along with a set of filterable categories to narrow down and find the exact users for whom they are looking.

All is well...right?

***WRONG***

A user tries to access the survey on a mobile device. The mobile browser renders above the fold HTML before populating the page with data from the endpoint described above... but the data never resolves. In fact, the page refreshes with the error message ***a problem occurred with this webpage, so it was reloaded.***

What is going on?

## Hitting the Mobile Hard Cap - 6MB

Apparently there is a hard cap on the amount of data a single webpage can ship to a mobile browser before the above error message occurs. ***That hard cap is 6MB.*** The endpoint described above was, at one point, shipping 12MB of data to each user.

I guess it kind of makes sense...webpages designed for mobile should not be shipping MBs of data, since most mobile are on cellular networks and pay for data by the GB.

## Lessons Learned

Page APIs, and as a wise man once told me *"don't ship the world."* Shipping every user on first hit of the endpoint so filtering can be done on the frontend just doesnt seem sustainable, particularly when all the data is on the order of MBs, 99% of which will never be used by the end user.

Save MBs. Ship less data. Save your users money.

All fine words to live by!
