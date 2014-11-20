# STARTX Application docker-images : Rockmongo
This container run Rockmongo on PHP server running under a fedora server. 

## Running from docker registry
	# docker run -it --name="rockmongo" startx/app-rockmongo

## Build and run from local Dockerfile
### Building docker image
Copy the sources to your docker host 

        mkdir startx-docker-images; 
        cd startx-docker-images;
        git clone https://github.com/startxfr/docker-images.git .

and build the container

        docker build -t startx/app-rockmongo Applications/rockmongo/

### Running local image

	# docker run -d -p 80:80 --name="rockmongo" startx/app-rockmongo

## Accessing server

	# firefox http://localhost/rockmongo

## Related Resources
* [Sources files](https://github.com/startxfr/docker-images/tree/master/Applications/rockmongo)
* [Github STARTX profile](https://github.com/startxfr/docker-images)
* [Docker registry for this container](https://registry.hub.docker.com/u/startx/app-rockmongo/)
* [Docker registry for Fedora](https://registry.hub.docker.com/u/fedora/)

