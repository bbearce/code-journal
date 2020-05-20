# Docker

Recap and cheat sheet: 

**List Docker CLI commands**

```
docker 
docker container --help 
```

**Display Docker version and info**

```
docker --version 
docker version 
docker info 
```

**Execute Docker image**

```
docker run hello-world 
```

**List Docker images** 

```
docker image ls 
docker image ls --all 
docker image ls -aq 
```

**List Docker containers (running, all, all in quiet mode)**

```
docker container ls 
docker container ls --all 
docker container ls –aq 
``` 

**List Docker services**
```
docker service ls 
docker stack services getstartedlab 
```

**Tasks** *Note the addition of '_web' to the service name*

```
docker service ps getstartedlab_web 
```
> Note: container ls will show you the tasks running as well 

```
docker container ls 
```

## Run without sudo

To create the docker group and add your user:

1. Create the docker group:
```$ sudo groupadd docker```

2. Add your user to the docker group.
```$ sudo usermod -aG docker $USER```

3. Log out and log back in so that your group membership is re-evaluated. If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
> On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.
On Linux, you can also run the following command to activate the changes to groups:
```$ newgrp docker ```

4. Verify that you can run docker commands without sudo.

```$ docker run hello-world```

## Useful Commands

Use ```docker container inspect 4ca8ce46f817``` to inspect a container. You will get a json dump of characteristics that are super useful.

Example:
```
[
    {
        "Id": "4ca8ce46f8170fc5c5eeb93bc27e8f84c2e8b32ddafc8e748a124e24fc8ff455",
        "Created": "2019-09-26T19:10:42.524604801Z",
        "Path": "python",
        "Args": [
            "predict.py"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2019-09-26T19:10:43.536396229Z",
            "FinishedAt": "2019-09-27T01:41:30.393256193Z"

.
.
.there is a lot more.
```

## Useful Flags

*-v|--volume[=[[HOST-DIR:]CONTAINER-DIR[:OPTIONS]]]*

```
          Create a bind mount. If you specify, -v /HOST-DIR:/CONTAINER-DIR, Docker 
          bind mounts /HOST-DIR in the host to /CONTAINER-DIR in the Docker 
          container. If 'HOST-DIR' is omitted,  Docker automatically creates the new 
          volume on the host.  The OPTIONS are a comma delimited list and can be: 

              · [rw|ro] 
              · [z|Z] 
              · [[r]shared|[r]slave|[r]private] 
              · [delegated|cached|consistent] 
              · [nocopy] 
```

*-d, --detach=true|false*

```
          Detached mode: run the container in the background and print the new container ID. The default is false. 
```

*-i, --interactive=true|false*

```
          Keep STDIN open even if not attached. The default is false. When set to true, keep stdin open even if not attached. 
```

*-t, --tty=true|false*

```
          Allocate a pseudo-TTY. The default is false. When set to true Docker can allocate a pseudo-tty and attach to the standard input of any container. This can be used, for example, to run a throwaway interactive shell. The default is false. The -t option is incompatible with a redirection of the docker client standard input. 
```

*--name=" "*

```
          Assign a name to the container 

       The operator can identify a container in three ways: 
  
       │Identifier type       │ Example value │ 
       │UUID long identifier  │ "f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778" │ 
       │UUID short identifier │ "f78375b1c487"│ 
       │Name                  │ "evil_ptolemy"|   
```

*--restart=" "*
To configure the restart policy for a container, use the ```--restart``` flag when using the docker run command. The value of the ```--restart``` flag can be any of the following:

| Flag           | Description                                                                                                                                                                                                                                                                                                                        |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| no             | Do not automatically restart the container. (the default)                                                                                                                                                                                                                                                                          |
| on-failure     | Restart the container if it exits due to an error, which manifests as a non-zero exit code.                                                                                                                                                                                                                                        |
| always         | Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in  [restart policy details](https://docs.docker.com/config/containers/start-containers-automatically/#restart-policy-details)) |
| unless-stopped | Similar to  ```always```, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts.                                                                                                                                                                                |


## Managing and Removing 

Good [discussion](https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/)

**Summary**
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
 
## Investigating and playing around 

**DockerHub:**

Image specifications: 

* Account: bbearce  
* Repo: <repo example>  
* Nomenclature: bbearce/<repo example>:<tag>  

 

To start with a clean slate: 

* Stop all containers: 
    1. ```docker container rm $(docker container ls -a -q)```         # Remove all containers 
    2. ```docker image rm $(docker image ls -a -q)```   # Remove all images from this machine 

If you want to run an instance of an image issue this command(this will download it if you don't have the image): 

```
docker run -it -d ashok/pyashokproj bin/bash 
```

Keep in mind this is different than getting inside a running container. For this to work the container needs to be already running: 

```
docker exec -it <container-name/ID> bash 
```

Some dockers can be started and stopped indefinitely either because they are a web server or an OS image(Ubuntu). Others based on things like pythonX.X can't be started up once stopped. A lot of customization and investigation can only really be done inside the running docker. So in order to get in we need to one of two things depending on the docker: 

*OS or web server type docker:*

Use ```docker container –ls a``` to find the stopped container id. Next use ```docker start <container id>```  to start the container again. Now use ```docker ps``` to see that it is running 

*Python or language image:*

If you attempt to start the stopped container it will run for a brief second like it already did upon instantiation and then stop. We have to explicitly use ```docker run -it -d <image> bin/bash```. This needs all of those flags (itd) in order to start a container in an interactive session, pipe the terminal from the docker to your local terminal and to run in detached mode. Now that it is running indefinitely we can run ```docker exec -it <container-name/ID> bash``` and actually connect to it. 

*Start a docker container:* 

```docker start <container-name/ID> ```

*Stop a docker container:*

```docker stop <container-name/ID> ```
 

Take a container and make a new image from it (Example): 

```docker run -itd codalab/codalab-legacy bash```

> Note: Don't forget the -d 

This downloads a new image and starts an instance. It doesn't have the python package 'seaborn' installed in the python so we are going to add it. We started a container and it is running in detached mode. Now let's connect to it. Use ```docker ps``` to get the container's id: 

```docker  exec -it d7ef724c4309 bash```  

You should be looking at the prompt: 

```root@f907b9e5d9a6:/#  ```

Follow these instructions 

```root@f907b9e5d9a6:/# pip install seaborn```  
...python is importing stuff... 


```root@f907b9e5d9a6:/# python```

```python
>>> import seaborn 
>>> 

```

You can see we installed seaborn. Now exit the docker ```root@f907b9e5d9a6:/# exit```. We need to build it to an image: 

```docker commit d7ef724c4309 bbearce/codalab:legacy ```

We just made an image and tagged it with this identifier ```bbearce/codalab:legacy``` and we can see it if we execute: 

```
$ docker images 
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE 
bbearce/codalab          legacy              c2d04ddb825a        3 seconds ago       2.65GB 
codalab/codalab-legacy   latest              432ce2829707        20 months ago       2.64GB 
```

Now let's push it online (Docker Hub) as an optional last step: 

```docker login ```

```docker push bbearce/codalab:legacy ```

 

Another way to share is with: 

```docker save ```

```docker load ```

[save/load docs](https://docs.docker.com/engine/reference/commandline/save/ )
 
## Internet issues

>Source [medium.com](https://medium.com/@faithfulanere/solved-docker-build-could-not-resolve-archive-ubuntu-com-apt-get-fails-to-install-anything-9ea4dfdcdcf2)

I had issues while connected to vpn. It was solved once I disconnected...