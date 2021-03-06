# Docker OS Images : MONGO

Simple container used to deliver document-oriented database
Run [mongodb daemon](https://www.mongodb.org/) under a container 
based on [startx/fedora container](https://hub.docker.com/r/startx/fedora)

| [![Build Status](https://travis-ci.org/startxfr/docker-images.svg)](https://travis-ci.org/startxfr/docker-images) | [Dockerhub Registry](https://hub.docker.com/r/startx/sv-mongo/) | [Sources](https://github.com/startxfr/docker-images/Services/mongo)             | [STARTX Profile](https://github.com/startxfr) | 
|-------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|---------------------------------------------------------------------------------|-----------------------------------------------|

## Available flavours

* `:latest` : Fedora core 23 + MongoDB 3.0.7 
* `:fc23` : Fedora core 23 + MongoDB 3.0.7  
* `:fc22` : Fedora core 22 + MongoDB 
* `:fc21` : Fedora core 21 + MongoDB 
* `:centos7` : CentOS 7 + MongoDB 
* `:centos6` : Centos 6 + MongoDB 

## Running from dockerhub registry

* with `docker` you can run `docker run -it --name="service-mongo" startx/sv-mongo` from any docker host
* with `docker-compose` you can create a docker-compose.yml file with the following content
```
service:
  image: startx/sv-mongo:latest
  container_name: "service-mongo"
  environment:
    CONTAINER_TYPE: "service"
    CONTAINER_SERVICE: "mongo"
    CONTAINER_INSTANCE: "service-mongo"
  volumes:
    - "/tmp/container/logs/mongo:/logs"
    - "/tmp/container/mongo:/data"
```

## Docker-compose in various situations

* sample docker-compose.yml linked to host port 1000
```
service:
  image: startx/sv-mongo:latest
  container_name: "service-mongo"
  environment:
    CONTAINER_INSTANCE: "service-mongo"
  ports:
    - "1000:27017"
```
* sample docker-compose.yml with port exposed only to linked services
```
service:
  image: startx/sv-mongo:latest
  container_name: "service-mongo"
  environment:
    CONTAINER_INSTANCE: "service-mongo"
  expose:
    - "27017"
```
* sample docker-compose.yml using data container
```
data:
  image: startx/fedora:latest
  container_name: "service-mongo-data"
  environment:
    CONTAINER_INSTANCE: "service-mongo-data"
service:
  image: startx/sv-mongo:latest
  container_name: "service-mongo"
  environment:
    CONTAINER_INSTANCE: "service-mongo"
  volume_from:
    - data:rw
```

## Using this image in your own container

You can use this Dockerfile template to start a new personalized container based on this container. Create a file named Dockerfile in your project directory and copy this content inside. See [docker guide](http://docs.docker.com/engine/reference/builder/) for instructions on how to use this file.
 ```
FROM startx/sv-mongo:latest
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
| DATA_PATH                 | `auto`   | `auto`    | default set to /data and used as a volume mountpoint

## Exposed port

| Port  | Description                                                              |
|-------|--------------------------------------------------------------------------|
| 27017 | standard mongodb network port used for non encrypted traffic

## Exposed volumes

| Container directory  | Description                                                              |
|----------------------|--------------------------------------------------------------------------|
| /logs                | log directory used to record container and mongo logs
| /data                | data directory served by mongo. If empty will be filled with app on startup. In other case use content from mountpoint or data volumes

## Testing the service

access to the running webserver with your favorites browser `mongo localhost:27017`. Change port and hostname according to your current configuration

## For advanced users

You want to use this container and code to build and create locally this container, follow theses instructions.

This section will help you if you want to :
* Get latest version of this service container
* Enhance container content by adding instruction in Dockefile before build step

You must have a working environment with the source code of this repository. Read and follow [how to setup your working environment](https://github.com/startxfr/docker-images#setup-your-working-environment-mandatory) to get a working directory. The following instructions assume you are at the top level of your working directory.

### Build & run a container using `docker`

1. Jump into the container directory with `cd Services/mongo`
2. Build the container using `docker build -t sv-mongo .`
3. Run this container 
  1. Interactively with `docker run -p 27017:27017 -v /logs -it sv-mongo`. If you add a second parameter (like `/bin/bash`) to will run this command instead of the default entrypoint. Usefull to interact with this container (ex: `/bin/bash`, `/bin/ps -a`, `/bin/df -h`,...) 
  2. As a daemon with `docker run -p 27017:27017 -v /logs -d sv-mongo`


### Build & run a container using `docker-compose`

1. Jump into the container directory with `cd Services/mongo`
2. Run this container 
  1. Interactively with `docker-compose up` Startup logs appears and escaping this command stop the container
  2. As a daemon with `docker-compose up -d`. Container startup logs can be read using `docker-compose logs`

If you experience trouble with port already used, edit docker-compose.yml file and change port mapping
