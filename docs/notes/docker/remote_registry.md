# Remote Registry

> Courtesy of [docs.docker.com](https://docs.docker.com/registry/deploying/)

## Deploy a registry server
Before you can deploy a registry, you need to install Docker on the host. A registry is an instance of the ```registry``` image, and runs within Docker.

## Run a local registry
Use a command like the following to start the registry container:
```bash
$ docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

The registry is now ready to use.

> Warning: These first few examples show registry configurations that are only appropriate for testing. A production-ready registry must be protected by TLS and should ideally use an access-control mechanism. Keep reading and then continue to the configuration guide to deploy a production-ready registry.

## Copy an image from Docker Hub to your registry
You can pull an image from Docker Hub and push it to your registry. The following example pulls the ubuntu:16.04 image from Docker Hub and re-tags it as my-ubuntu, then pushes it to the local registry. Finally, the ubuntu:16.04 and my-ubuntu images are deleted locally and the my-ubuntu image is pulled from the local registry.

1. Pull the ubuntu:16.04 image from Docker Hub.

```bash
$ docker pull ubuntu:16.04
```

2. Tag the image as localhost:5000/my-ubuntu. This creates an additional tag for the existing image. When the first part of the tag is a hostname and port, Docker interprets this as the location of a registry, when pushing.

```bash
$ docker tag ubuntu:16.04 localhost:5000/my-ubuntu
```

3. Push the image to the local registry running at localhost:5000:

```bash
$ docker push localhost:5000/my-ubuntu
```

4. Remove the locally-cached ubuntu:16.04 and localhost:5000/my-ubuntu images, so that you can test pulling the image from your registry. This does not remove the localhost:5000/my-ubuntu image from your registry.

```bash
$ docker image remove ubuntu:16.04
$ docker image remove localhost:5000/my-ubuntu
```

5. Pull the localhost:5000/my-ubuntu image from your local registry.
```bash
$ docker pull localhost:5000/my-ubuntu
```

## Stop a local registry
To stop the registry, use the same docker container stop command as with any other container.
```bash
$ docker container stop registry
```

To remove the container, use docker container rm.
```bash
$ docker container stop registry && docker container rm -v registry
```

## SSL and HTTPS

