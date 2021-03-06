# Docker OS Images : POSTGRESQL

Simple container used to deliver highly reliable and configurable transactional database service using postgresql opensource project.
Run [postgres daemon](http://www.postgresql.org/) under a container 
based on [startx/fedora container](https://hub.docker.com/r/startx/fedora)

| [![Build Status](https://travis-ci.org/startxfr/docker-images.svg)](https://travis-ci.org/startxfr/docker-images) | [Dockerhub Registry](https://hub.docker.com/r/startx/sv-postgres/) | [Sources](https://github.com/startxfr/docker-images/Services/postgres)             | [STARTX Profile](https://github.com/startxfr) | 
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------------------------------|-----------------------------------------------|

## Available flavours

* `:latest` : Fedora core 23 + PostgreSQL 9.4.5 
* `:fc23` : Fedora core 23 + PostgreSQL 9.4.5  
* `:fc22` : Fedora core 22 + PostgreSQL 
* `:fc21` : Fedora core 21 + PostgreSQL 
* `:centos7` : CentOS 7 + PostgreSQL 
* `:centos6` : Centos 6 + PostgreSQL 

## Running from dockerhub registry

* with `docker` you can run `docker run -it --name="service-postgres" startx/sv-postgres` from any docker host
* with `docker-compose` you can create a docker-compose.yml file with the following content
```
service:
  image: startx/sv-postgres:latest
  container_name: "service-postgres"
  ports:
    - "5432:5432"
  environment:
    CONTAINER_TYPE: "service"
    CONTAINER_SERVICE: "postgres"
    CONTAINER_INSTANCE: "service-postgres"
    POSTGRESQL_ROOT_PASSWORD: "rootpassword"
    POSTGRESQL_USER: "user-test"
    POSTGRESQL_PASSWORD: "pwd-test"
    POSTGRESQL_DATABASE: "db_test"
  volumes:
    - "/tmp/container/logs/postgres:/logs"
    - "/tmp/container/postgres:/data"
```

## Docker-compose in various situations

* sample docker-compose.yml linked to host port 1000
```
service:
  image: startx/sv-postgres:latest
  container_name: "service-postgres"
  environment:
    CONTAINER_INSTANCE: "service-postgres"
  ports:
    - "1001:5432"
```
* sample docker-compose.yml with port exposed only to linked services
```
service:
  image: startx/sv-postgres:latest
  container_name: "service-postgres"
  environment:
    CONTAINER_INSTANCE: "service-postgres"
  expose:
    - "5432"
```
* sample docker-compose.yml using data container
```
data:
  image: startx/fedora:latest
  container_name: "service-postgres-data"
  environment:
    CONTAINER_INSTANCE: "service-postgres-data"
service:
  image: startx/sv-postgres:latest
  container_name: "service-postgres"
  environment:
    CONTAINER_INSTANCE: "service-postgres"
  volume_from:
    - data:rw
```

## Using this image in your own container

You can use this Dockerfile template to start a new personalized container based on this container. Create a file named Dockerfile in your project directory and copy this content inside. See [docker guide](http://docs.docker.com/engine/reference/builder/) for instructions on how to use this file.
 ```
FROM startx/sv-postgres:latest
#... your container specifications
CMD ["/bin/run.sh"]
```

## Environment variable

| Variable                  | Type     | Mandatory | Description                                                              |
|---------------------------|----------|-----------|--------------------------------------------------------------------------|
| CONTAINER_INSTANCE        | `string` | `yes`     | Container name. Should be uning to get fine grained log and application reporting
| CONTAINER_TYPE            | `string` | `no`      | Container family (os, service, application. could be enhanced 
| CONTAINER_SERVICE         | `string` | `no`      | Define the type of service or application provided
| POSTGRESQL_ROOT_PASSWORD  | `string` | `no`      | Root password used for this instance. Default will use an auto generated password displayed on startup
| POSTGRESQL_USER           | `string` | `no`      | If present, add a new user with this name
| POSTGRESQL_PASSWORD       | `string` | `no`      | Password associated to the new user declared with $POSTGRESQL_USER
| POSTGRESQL_DATABASE       | `string` | `no`      | If present, add a new database with this name
| LOADSQL_PATH              | `string` | `auto`    | Path used to find sql dump to import at startup
| HOSTNAME                  | `auto`   | `auto`    | Container unique id automatically assigned by docker daemon at startup
| LOG_PATH                  | `auto`   | `auto`    | default set to /logs and used as a volume mountpoint
| DATA_PATH                 | `auto`   | `auto`    | default set to /data and used as a volume mountpoint

## Exposed port

| Port  | Description                                                              |
|-------|--------------------------------------------------------------------------|
| 5432  | standard postgres network port used for sql communication

## Exposed volumes

| Container directory  | Description                                                              |
|----------------------|--------------------------------------------------------------------------|
| /logs                | log directory used to record container and postgres logs
| /data                | data directory served by postgres. If empty will be filled with database files on startup. In other case use content from mountpoint or data volumes

## Testing the service

access to the running service with mysql terminal `psql -h localhost -p 5432 -U <POSTGRESQL_USER>`. Change port and hostname according to your current configuration

## For advanced users

You want to use this container and code to build and create locally this container, follow theses instructions.

This section will help you if you want to :
* Get latest version of this service container
* Enhance container content by adding instruction in Dockefile before build step

You must have a working environment with the source code of this repository. Read and follow [how to setup your working environment](https://github.com/startxfr/docker-images#setup-your-working-environment-mandatory) to get a working directory. The following instructions assume you are at the top level of your working directory.

### Build & run a container using `docker`

1. Jump into the container directory with `cd Services/postgres`
2. Build the container using `docker build -t sv-postgres .`
3. Run this container 
  1. Interactively with `docker run -p 5432:5432 -v /logs -it sv-postgres`. If you add a second parameter (like `/bin/bash`) to will run this command instead of the default entrypoint. Usefull to interact with this container (ex: `/bin/bash`, `/bin/ps -a`, `/bin/df -h`,...) 
  2. As a daemon with `docker run -p 5432:5432 -v /logs -d sv-postgres`


### Build & run a container using `docker-compose`

1. Jump into the container directory with `cd Services/postgres`
2. Run this container 
  1. Interactively with `docker-compose up` Startup logs appears and escaping this command stop the container
  2. As a daemon with `docker-compose up -d`. Container startup logs can be read using `docker-compose logs`

If you experience trouble with port already used, edit docker-compose.yml file and change port mapping
