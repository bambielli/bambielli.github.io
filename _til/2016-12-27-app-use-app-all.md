---
title:  "app.use and app.all"
date:   2016-12-27 11:13:00
category: til
tags: [express, node, javascript]
---

TIL the difference between `app.use` and `app.all` in the context of an express application.

I've been spending some free time over the holidays working on an app idea. I figured since I hadn't built anything in express before, this was a good opportunity to do so. I was working on getting Oauth2 authentication to work (using passport.js and session storage using express-session) when I realized I still needed to implement a piece of middleware to check if the user was authenticated for API endpoints. Passport exposes a method on the `req` object called `isAuthenticated()` which returns a boolean.

### app.use vs app.all

`app.use` applies the specified middleware to the main app middleware stack. When attaching middleware to the main app stack, the order of attachment matters; if you attach middleware A before middleware B, middleware A will always execute first. You can specify a path for which a particular middleware is applicable. In the below example, "hello world" will always be logged before "happy holidays."

{%highlight javascript%}
const express = require('express')
const app = express()

app.use(function(req, res, next) {
  console.log('hello world')
  next()
})

app.use(function(req, res, next) {
  console.log('happy holidays')
  next()
})

{%endhighlight%}

`app.all` on the other hand will attach to the app's implicit router. `app.all` attaches a particular piece of middleware to all HTTP methods, and if attached in the main config file will globally apply the middleware to all requests made to your app. Like `app.use`, it is also possible to specify a path for which the middleware should be applied.

{%highlight javascript%}
const express = require('express')
const app = express()

app.all('/api/*', function(req, res, next) {
  console.log('only applied for routes that begin with /api')
  next()
})
{%endhighlight%}

`app.all` also accepts a regex as its path parameter. `app.use` does not accept a regex, but will automatically match all routes that extend the base route.

### Summary

All in all, `app.all` and `app.use` can be used to achieve similar results: `app.all('*')` and `app.use('/')` achieve the same thing!

The devil is in the details: where `app.use('/api')` will match both the route /app as well as any routes that extend /app, `app.all('/api/*')` will ONLY match routes that extend /api (i.e /api/resource, and NOT /api alone).

I wound up using `app.use('/api', ...)` to attach my authentication middleware to my app, so in my app.js config file I needed to make sure that it was positioned above the routers to ensure it would be run before any other callbacks associated with my api routes.

An additional thought I had: `app.use` is used for configuring middleware at your highest level "controller" for your application. Middlewares applied with app.use are executed before any logic associated with HTTP methods on sub-controllers controlled by routers. For this reason, once http methods are reached, middleware applied using app.use will not be executed. Since app.all attaches a piece of middleware to all HTTP methods, it will still be executed (in the order in which it is applied) during the HTTP method resoultion phase in sub controllers.
