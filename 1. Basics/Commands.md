# Managing images

```
docker search <image name>                           # To search the docker images from command line
docker pull <image name>                             # To pull a docker image from docker hub	
docker images                                        # To display the images on your local machine
docker history <image name>                          # To know the changes done to the image
docker inspect <image name>                          # To view the detaied information in JSON output
docker rmi <image name>                              # To remove a image from local machine
docker image prune                                   # To remove all unused images from local machine
```

# Running containers from an image

- Interative mode ( -it ): Once created, opens the termimal of the container. Suitable for editing/updating the information within container. Usually used with OS containers.
```	
  docker run -it nginx bash                          # To creates new container & enter inside the container
```
- Detached mode  ( -d ): Runs the image as containers in the background. Doesn't open a shell. Suitable for application containers
```	
  docker run -d nginx                                # To creates new container & detaches from the terminal you are working on (runs as a background process)
```

# Managing containers 
```
  docker ps                                          # To check all running containers 
  docker ps -a                                       # To check all running + exited/stopped containers
  docker stop <container id>                         # To stop a container 
  docker start <container id>                        # To start a stopped container 
  docker restart <container id>                      # To restart a container   
  docker inspect <container id>                      # To view detailed information about the continaer in JSON format
  docker rm <container id>                           # To remove a stopped/exited container
  docker rm -f <container id>                        # To remove a container forcefully though it is in running state 
  docker container prune                             # To remove all stopped/exited containers
```

# Getting inside a running container
```
  docker run -d nginx                                # Creates a new container in detached mode 
  docker ps                                          # Displays the running containers, note down the container id
  docker exec -it <container id> bash                # Opens the container terminal 
```

# Exit out of a container
```
  ctrl pq                                            # Graceful exit. It is recommended to use this command always
```  

# Expose the  applications running inside the container to external world (Through browser)
	
Services running inside a container can never be accessed direcltry, we always need to publish/expose them on to docker host while we create the containers. We can to publish/expose the services using -P (capital) OR -p (small) within the docker run command.

Ex: 
- -P (capital) -- docker will publish/expose the port number dynamically on docker host & maps with port running inside the container. Access the application in the browser via http://<docker host IP>:<exposed port>, Ex: http://52.14.62.88:32768
```
  docker run -d -P nginx                             # To create a new port mapping from docker host to the service inside the container 
  docker ps                                          # To check the container port 
```
  
- -p ( small )                                       # To explicitly assign a port on docker host & map to the port inside the container. Access the application in the browser via http://<docker host IP>:<exposed port>, Ex: http://52.14.62.88:1234 
```		
  docker run -d -p 1234:80 nginx                     # To explicitly assign a port map. The format is always port-on-docker-host : process-port-inside-container
  docker ps                                          # To check the container port 
```