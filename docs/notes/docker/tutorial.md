# Docker Command Sumamry

<iframe width="560" height="315" src="https://www.youtube.com/embed/Yy549qywTQQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## We will go over these fundamental commands:
```bash

docker build -t friendlyhello .  # Create image using this directory's Dockerfile  
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode  
docker container ls                                # List all running containers  
docker container ls -a             # List all containers, even those not running  
docker container stop <hash>           # Gracefully stop the specified container  
docker container kill <hash>         # Force shutdown of the specified container  
docker container rm <hash>        # Remove specified container from this machine  
docker container rm $(docker container ls -a -q)         # Remove all containers  
docker image ls -a                             # List all images on this machine  
docker image rm <image id>            # Remove specified image from this machine  
docker image rm $(docker image ls -a -q)   # Remove all images from this machine  
docker login             # Log in this CLI session using your Docker credentials  
docker tag <image> username/repository:tag  # Tag <image> for upload to registry  
docker push username/repository:tag            # Upload tagged image to registry  
docker run username/repository:tag                   # Run image from a registry  
```

## Lists images on machine
```bash
docker images
```

## Lists running containers
```bash
docker ps
```

## Lists all containers stopped or otherwise
```bash
docker ps -a
```
## Start a docker
```bash
docker run bbearce/docker_demo:from_docker_file
```


## Delete a container
```bash
docker container rm 3e64bfa61b7e
```

## Removes an image
```bash
docker image rm 7a59e09539e4
```

## Run an interactive session
```bash
docker run -it bbearce/docker_demo:from_docker_file bash
```

* ```-i``` - interactive sessoin
* ```-t``` - join docker terminal to yours
* ```bash``` - run *bash* inside the session instead of default program(s)

## Detach from container and leave running
```html
<cntrl p+q>
```
## To attach back to a docker container
```bash
docker attach silly_wright
```

or

```bash
docker attach <container_id>
```

##  Stop a running docker like this (you'll need to to delete it)
```bash
docker stop <container_id or name>
```

## To name a container
```--name=<name>```

```bash
docker run -it --name=auto_remove bbearce/docker_demo:from_docker_file bash
```


## Tag existing image as a different name (repo/image:tag)
```bash
docker tag bbearce/docker_demo:from_docker_file bbearce/docker_demo:delete_me
```

## Login to your dockerhub account
```bash
docker login --username=bbearce
```

## To push image online
```bash
docker push bbearce/docker_demo:from_docker_file
```


## To automatically remove container when exited
```bash
docker run -it --rm --name=auto_remove bbearce/docker_demo:from_docker_file bash
```

## Use bash flag *-v <host_machine_dir>:<docker_dir>* to mount the host folder inside the docker.
```bash
docker run -it --rm --name=auto_remove -v /home/bbearce/Desktop/Docker_Demo/mount:/app bbearce/docker_demo:from_docker_file bash
```

## from a Dockerfile

```bash
docker build -t bbearce/docker_demo:from_docker_file .
```

