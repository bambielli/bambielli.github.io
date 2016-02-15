---
layout: post
title:  "Service Design in Angular"
date:   2016-02-14 20:18:00
category: til
tags: [angularJS, design]
---

TIL how to design a service in Angular that encapsulates the actions necessary to supply data to a controller.

We are doing some refactoring of one of our API endpoints. We are implementing paging on the api to try and reduce load and data shipped to the user. This has involved significant work on both the endpoint itself, and on the components on the frontend that request data from the endpoint.

Currently our controllers make all of our API requests directly: on loading a controller, a set of scope attributes are examined to see if the required resources have been fetched, and if not, there is a method in a parent controller for the module that makes the requests and sets the data on rootScope. Furthermore, once data has been fetched, the logic for sorting, filtering, and parsing the data is also contained in the controller. Clearly there is a separation of concerns issue here, when the controller is doing both the requesting and post-processing of data before it is inevitably exposed to the template for rendering.

During this refactor of the api endpoint, we are encapsulating all of the data requesting and post-processing logic for this endpoint into a service. The service is the interface between the controller and the data it needs: the controller doesn't care about the implementation of the service, it just knows that when it needs data it can poll the service with a set of user-dictated parameters and the appropriate data will be returned. The design of this service was tricky for me to understand, but

The service internals are optimized to reduce the number of requests to the application server: once a chunk of data is requested, it is stored in browser memory by setting a local variable in the service. That way if the same data is requested again by the controller, another round trip to the server won't be necessary, further reducing load.

I've been learining a lot about service design during this refactor, and I'm sure I'll be adding a few more TILs to the list as I finish up over the next few days!