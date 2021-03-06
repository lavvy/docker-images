# Docker OS Images : OOCONV

Simple container used to document convertion and manipulation tools
Run [libreoffice](https://www.libreoffice.org/) as a daemon using [dagwieers unoconv](https://github.com/dagwieers/unoconv) under a container 
based on [startx/fedora container](https://hub.docker.com/r/startx/fedora)

| [![Build Status](https://travis-ci.org/startxfr/docker-images.svg)](https://travis-ci.org/startxfr/docker-images) | [Dockerhub Registry](https://hub.docker.com/r/startx/sv-ooconv/) | [Sources](https://github.com/startxfr/docker-images/Services/ooconv)             | [STARTX Profile](https://github.com/startxfr) | 
|-------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------|

## Available flavours

* `:latest` : Fedora core 23 + unoconv 0.7 + LibreOffice 5.0.3.2
* `:fc23` : Fedora core 23 + unoconv 0.7 + LibreOffice 5.0.3.2
* `:fc22` : Fedora core 22 + unoconv 
* `:fc21` : Fedora core 21 + unoconv 
* `:centos7` : CentOS 7 + unoconv 
* `:centos6` : Centos 6 + unoconv 

## Running from dockerhub registry

* with `docker` you can run `docker run -it --name="service-ooconv" startx/sv-ooconv` from any docker host
* with `docker-compose` you can create a docker-compose.yml file with the following content
```
service:
  image: startx/sv-ooconv:latest
  container_name: "service-ooconv"
  environment:
    CONTAINER_TYPE: "service"
    CONTAINER_SERVICE: "ooconv"
    CONTAINER_INSTANCE: "service-ooconv"
  volumes:
    - "/tmp/container/logs/ooconv:/logs"
```

## Docker-compose in various situations

* sample docker-compose.yml linked to host port 1000
```
service:
  image: startx/sv-ooconv:latest
  container_name: "service-ooconv"
  environment:
    CONTAINER_INSTANCE: "service-ooconv"
  ports:
    - "1000:2002"
```
* sample docker-compose.yml with port exposed only to linked services
```
service:
  image: startx/sv-ooconv:latest
  container_name: "service-ooconv"
  environment:
    CONTAINER_INSTANCE: "service-ooconv"
  expose:
    - "2002"
```

## Using this image in your own container

You can use this Dockerfile template to start a new personalized container based on this container. Create a file named Dockerfile in your project directory and copy this content inside. See [docker guide](http://docs.docker.com/engine/reference/builder/) for instructions on how to use this file.
 ```
FROM startx/sv-ooconv:latest
#... your container specifications
CMD ["/bin/run.sh"]
```

## Environment variable

| Variable                  | Type     | Mandatory | Description                                                              |
|---------------------------|----------|-----------|--------------------------------------------------------------------------|
| CONTAINER_INSTANCE        | `string` | `yes`     | Container name. Should be uning to get fine grained log and application reporting
| CONTAINER_TYPE            | `string` | `no`      | Container family (os, service, application. could be enhanced 
| CONTAINER_SERVICE         | `string` | `no`      | Define the type of service or application provided
| HOSTNAME                  | `auto`   | `auto`    | Container unique id automatically assigned by docker daemon at startup
| LOG_PATH                  | `auto`   | `auto`    | default set to /logs and used as a volume mountpoint

## Exposed port

| Port  | Description                                                              |
|-------|--------------------------------------------------------------------------|
| 2002  | network port used to communicate with unoconv service

## Exposed volumes

| Container directory  | Description                                                              |
|----------------------|--------------------------------------------------------------------------|
| /logs                | log directory used to record container and ooconv logs

## Testing the service

access to the running unoconv service with unoconv client `unoconv -s localhost -p 2002`. Change port and hostname according to your current configuration

## For advanced users

You want to use this container and code to build and create locally this container, follow theses instructions.

This section will help you if you want to :
* Get latest version of this service container
* Enhance container content by adding instruction in Dockefile before build step

You must have a working environment with the source code of this repository. Read and follow [how to setup your working environment](https://github.com/startxfr/docker-images#setup-your-working-environment-mandatory) to get a working directory. The following instructions assume you are at the top level of your working directory.

### Build & run a container using `docker`

1. Jump into the container directory with `cd Services/ooconv`
2. Build the container using `docker build -t sv-ooconv .`
3. Run this container 
  1. Interactively with `docker run -p 2002:2002 -v /logs -it sv-ooconv`. If you add a second parameter (like `/bin/bash`) to will run this command instead of the default entrypoint. Usefull to interact with this container (ex: `/bin/bash`, `/bin/ps -a`, `/bin/df -h`,...) 
  2. As a daemon with `docker run -p 2002:2002 -v /logs -d sv-ooconv`


### Build & run a container using `docker-compose`

1. Jump into the container directory with `cd Services/ooconv`
2. Run this container 
  1. Interactively with `docker-compose up` Startup logs appears and escaping this command stop the container
  2. As a daemon with `docker-compose up -d`. Container startup logs can be read using `docker-compose logs`

If you experience trouble with port already used, edit docker-compose.yml file and change port mapping
