# Docker Commands

## run -  Start a Container

```
docker run nginx
```

This command will run an instance of then nginx application on the Docker host if it already exists.

If the image is not present on the host, it will go out ti Docker hub and pull the image down.

This is only done for the first time. For subsequent executions, the same image will be used.

## ps - List Containers

```
docker ps
```

This command lists all running containers and some basic information about them such as the container ID, the name of the image we used to run the containers, the current status and the name of the container.

Each container automatically gets a random ID and name created for it by Docker.

To see all containers running or not, use the -a option

```
docker ps -a
```

This outputs all running as well as previously stopped or exited containers

## stop - Stop a Container
```
docker stop <container-name>
```

This command stops a running container. You must provide either the container ID or the container name.

On success, you will see the `container-name` printed out and running `docker ps` again, will show no running containers.

```
docker ps -a
```

This command shows the stopped containers in an exited state

## rm - Remove a Container
```
docker rm <container-name>
```

This command removes a stopped or exited container permanently. On success, it prints the name back.

Run `docker ps -a` to verify that it is no longer present.

## images - List images
```
docker images
```

This provides a list of available images and their sizes.

## rmi - Remove Images
```
docker rmi
```

Used to remove images that are no longer in use. 

You must ensure that no containers are running off of that image before attempting to remove the image. You must stop and delete all dependent containers to be able to delete an image.

## pull - Download an Image
```
docker pull nginx
```

This command pulls an image, but does not run the container.

## exec - Execute a Command
```
docker exec
```

Used to execute a command on a docker container