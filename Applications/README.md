# STARTX Docker-Images Application Repository

This directory host various Open Source applications into docker containers based on [startx/fedora container](https://hub.docker.com/r/startx/fedora)

You can use it to instanciate new container from the dockerhub public registry 
or as a parent container in your own container's. 
Each container is provided with various underlying OS version based on CentOS or 
Fedora Linux distribution.

If you're used to work with redhat like and distribution, you would prefer start working
from a container based on this distribution rather than ubuntu or debian like distributions.
OS, applications and application you will find here will be closer to your favorite distrbution

Please **read each container manual** for more information on how you can use it in 
your project and start using stable or experimental applications in your applications

| [![Build Status](https://travis-ci.org/startxfr/docker-images.svg)](https://travis-ci.org/startxfr/docker-images) | [Dockerhub Registry](https://hub.docker.com/r/startx) | [Sources](https://github.com/startxfr/docker-images/)             | [STARTX Profile](https://github.com/startxfr) | 
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------|

## Available applications

| Service         | Docker Hub repository                                                     | Fedora flavours                   | CentOS flavours | Doc                                        | 
|-----------------|---------------------------------------------------------------------------|-----------------------------------|-----------------|--------------------------------------------|
| PhpMyAdmin      | [`startx/app-pma`](https://hub.docker.com/r/startx/app-pma)               | `:latest` `:20` `:21` `:22` `:23` | `:6` `:7`       | [manual](pma/README.md)                    | 
| RockMongo       | [`startx/app-rockmongo`](https://hub.docker.com/r/startx/app-rockmongo)   | `:latest` `:20` `:21` `:22` `:23` | `:6` `:7`       | [manual](rockmongo/README.md)              | 


## Running from dockerhub registry

| Applications        | Command                                                              |
|---------------------|----------------------------------------------------------------------|
| **PhpMyAdmin**      | `docker run -d -p 80:80 --name="pma" startx/app-pma`                 | 
| **RockMongo**       | `docker run -d -p 80:80 --name="rockmongo" startx/app-rockmongo`     | 

## Container flavours

| Service    | Docker Hub repository                                                   | Fedora                                    | CentOS                |
|------------|-------------------------------------------------------------------------|-------------------------------------------|-----------------------|
| PhpMyAdmin | [`startx/app-pma`](https://hub.docker.com/r/startx/app-pma)             | `:latest` `:fc20` `:fc21` `:fc22` `:fc23` | `:centos6` `:centos7` |
| RockMongo  | [`startx/app-rockmongo`](https://hub.docker.com/r/startx/app-rockmongo) | `:latest` `:fc20` `:fc21` `:fc22` `:fc23` | `:centos6` `:centos7` |
