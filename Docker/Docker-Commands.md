
#### Basic Commands of docker
	
	To find version                              ``$docker --verion
	To run a container                           ``$docker run <image-name>:ver  
	To Pull the image from the hub               ``$docker pull <image-name>
	To get names of images in local              ``$docker images
	To see running containers                    ``$docker ps -a
	To stop a running container                  ``$docker stock <container-id>
	To Remove the container                      ``$docker rm <container-id>
	To view logs of the container                ``$docker logs <container-id>
	To execute commands inside container         ``$docker exec -it <container-id>


```shell
#To build an Image from a docker file
$docker build -t <image-name> .
    #-t tells the name of the image
    # . tells docker to search for Dockerfile in the current Directory

#To Run the Container from the build image
$docker run -d -p 5000:4000 <image-name>
	#-d deatached from the terminal
	#-p 5000:4000 Map port 5000 of the machine to port 4000 in the container

#To run commands inside the container 
$docker exec -it <conainer-name> bash
	#-it refers to interactive mode

#To setup a persistent Data mode 
$docker run -d --name my-sql -e MYSQL_ROOT_PASSWORD=password -v mysql-data:/va r/lib/mysql mysql
	#-v mysql-data:/var/lib/mysql create a volume called mysql-data. 	
$docker run -d -v mydata:/app/data myapp
	#Mounts a volume named mydata to the dir /app/data inside the container
```


# Docker Commands for Building and Running Containers

## Build an Image from a Dockerfile
```shell
docker system prune -f --volumes    --- to delete container
docker-compose down -v
````