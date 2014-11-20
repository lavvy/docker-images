# STARTX Services docker-images : PostgreSQL Server
This container run postgres on fedora server. 

## Running from docker registry

	# docker run -d -p 5432:5432 --name="postgres" startx/sv-postgres
	# when linked to another container
	# docker run -d --name="postgres" startx/sv-postgres
	# docker run -d --name="php" --link postgres:postgres startx/sv-php

## Build and run from local Dockerfile
### Building docker image
Copy the sources to your docker host 

        mkdir startx-docker-images; 
        cd startx-docker-images;
        git clone https://github.com/startxfr/docker-images.git .

and build the container

        docker build -t sv-postgres Services/postgres/

### Running local image

	# docker run -d -p 5432:5432 --name="postgres" sv-postgres

## Accessing server

	# psql -U postgres -h localhost -p 5432

## Related Resources
* [Sources files](https://github.com/startxfr/docker-images/tree/master/Services/postgres)
* [Github STARTX profile](https://github.com/startxfr/docker-images)
* [Docker registry for this container](https://registry.hub.docker.com/u/startx/sv-postgres/)
* [Docker registry for Fedora](https://registry.hub.docker.com/u/fedora/)