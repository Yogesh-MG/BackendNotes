
#### Basic Commands of docker
	
	To find version                              ``$docker --verion
	To run a container                           ``$docker run <image-name>:ver  
	To Pull the image from the hub               ``$docker pull <image-name>
	To get names of images in local              ``$docker images
	To see running containers                    ``$docker ps -a
	To stop a running container                  ``$docker stock <container-id>
	To Remove the container                      ``$docker rm <container-id>
	To view logs of the container                ``$docker logs <container-id>


```shell
#To build an Image from a docker file
$docker build -t <image-name> .
    #-t tells the name of the image
    # . tells docker to search for Dockerfile in the current Directory

#To Run the Container from the build image
$docker run -d -p 5000:4000 <image-name>
	#-d deatached from the terminal
	#-p 5000:4000 Map port 5000 of the machine to port 4000 in the container

```