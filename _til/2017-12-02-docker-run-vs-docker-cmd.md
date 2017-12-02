---
title:  "RUN vs CMD in Dockerfile"
date:   2017-11-18 13:35:00
category: til
tags: ["docker", "container", "image", "run", "cmd", "dockerfile"]
---

TIL the difference between RUN and CMD when defining an image via a dockerfile.

## RUN

[`RUN`][run]{:target="_blank"} is used when writing the buildscript for an image via dockerfile, to define steps that will be executed when you build an image from the file using `docker build -t imageName:tagName .` A dockerfile can have many RUN steps defined.

RUN steps can be used to modify the state of the image being constructed. For example: if I have node and npm available in my docker image, and have copied a package.json into the image, I can use `RUN npm install` to make the dependencies defined by the package.json available in my container.

## CMD

[`CMD`][cmd]{:target="_blank"} is used do define a command that will be executed when the image is run in a container. There can only be **one** CMD defined as part of your dockerfile.

If your image contains application code for an `express.js` server, the CMD step in your dockerfile would be `CMD node server.js` or whatever file is necessary to kick off your server.

CMD can be overridden from the command line by passing a new command to be run as an option. For example, if the default CMD defined in the dockerfile is `CMD node server.js`, but instead you just want to exec into the container, try `docker run imageName:tagName /bin/sh/`

[run]: https://docs.docker.com/engine/reference/builder/#run
[cmd]: https://docs.docker.com/engine/reference/builder/#cmd