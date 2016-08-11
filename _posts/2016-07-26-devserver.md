---
title:  "slidewiki/devserver - Run the SlideWiki Platform out-of-the-box"
date:   2016-07-26 18:00:00 +0000
layout: post
---
The SlideWiki Platform will be delivered as a ready-to-run Docker container. 
But Docker can also help in simplifying development. Based on the runtime
image for the platform we have developed a dynamic version that makes it 
possible to edit the source code and hot-deploy your changes each time you
save it. 
The image that is available from [Docker Hub](https://hub.docker.com/r/slidewiki/devserver/) as `slidewiki/devserver` mounts
the source code of the platform from your host filesystem and monitors changes,
restarting the server each time a file is updated. This gives you the ability 
to use your development environment to develop in SlideWiki without the need
to install the NodeJS ecosystem. You only need the source code and the devserver
image in order to run the platform.

## Starting the devserver
When starting the devserver image you need to provide the **absolute** path 
to the slidewiki-platform project directory (pointing to volume locations 
in Docker always needs absolute paths). Also you need to provide a port on
your host to bin the container's nodeJS application port to. A typpical 
invocation looks like this:

`docker run -it --rm --name swdev -p 3000:3000 -v /absolute/path/to/project:/nodeApp slidewiki/devserver`

by default*, you should be able to see the running platform at http://localhost:3000
* If you are on Mac OS, you still might need to check the settings for the environmental variable `DOCKER_HOST` and use the correct host like e.g. http://192.168.99.101:3000

## Microservices
In the `/configs` directory in the project there needs to be a file that 
configures the location of the microservices (`microservices.js`). We provide a 
file called `microservices.sample.js` that lets the platform use the microservice
instances at our testing servers. The container startup script will copy the sample
file to the configuration file in case that `microservices.js` is not exsting. So
unless you want to use your own microservice instances **you just don't need to care about** :)

## Issues with Windows
Most displeasingly the devserver image does not work on Windows hosts as of now. The
reason is that the container runs `npm install` on startup in the project directory. 
Since the project directory resides in the Windows host filesystem it fails to create
symlinks when installing the dependencies. The bug is known and filed as [SWIK-286](https://slidewiki.atlassian.net/browse/SWIK-286)

[Benjamin Wulff](https://github.com/bwulff)
