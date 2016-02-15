---
layout: post
title:  "Service Design in Angular"
date:   2016-02-14 20:18:00
category: til
tags: [angular, design]
---

TIL how to design a service in Angular that encapsulates the actions necessary to supply data to a controller.

We are doing some refactoring of one of our API endpoints at work. This endpoint ships far too much data to the front-end, which is causing issues for our application infrastructure during periods of heavy load. Alleviating this resource bottleneck has involved significant refactoring of both the endpoint itself, and of the front-end components that require data from this endpoint.

Currently our controllers make all of our API requests directly: on loading a controller, a set of scope attributes are polled to see if required data have been fetched, and if not, there is a method in a parent controller for the module that makes these requests and sets the resulting data on scope. The logic for sorting, filtering, and parsing the fetched data is also contained in the controller. __Clearly there is a separation of concerns issue here...__ the controller is doing both the requesting and post-processing of data before it is inevitably exposed to the template for rendering.

To rectify this, we are __encapsulating all of the requesting and post-processing logic for this endpoint into an angular service.__ The service acts as the interface between the controller and the data it needs: the controller doesn't care about the implementation of the service, it just knows that when it needs data it can poll the service with a set of user-dictated parameters and the appropriate data will be returned. This increases code reusability, since we can just inject this service in to other controllers that need to interact with the endpoint.

The service internals are optimized to reduce the number of requests to the application server: once a chunk of data is requested, it is stored in browser memory by assigning it to a local private variable in the service. This way if the controller makes another request for data, and the appropriate data has already been fetched, the service will just pull the data from “memory” instead of making another round trip to the server.

I've been learning a lot about service design during this refactor, and I'm sure I'll be adding a few more TILs to the list as I finish up over the next few days!