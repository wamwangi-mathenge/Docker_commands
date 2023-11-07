# Docker Commands

### run -  Start a Container

```
docker run nginx
```

This command will run an instance of then nginx application on the Docker host if it already exists.

If the image is not present on the host, it will go out ti Docker hub and pull the image down.

This is only done for the first time. For subsequent executions, the same image will be used.

### ps - List Containers

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

### stop - Stop a Container
```
docker stop <container-name>
```

This command stops a running container. You must provide either the container ID or the container name.

On success, you will see the `container-name` printed out and running `docker ps` again, will show no running containers.

```
docker ps -a
```

This command shows the stopped containers in an exited state

### rm - Remove a Container
```
docker rm <container-name>
```

This command removes a stopped or exited container permanently. On success, it prints the name back.

Run `docker ps -a` to verify that it is no longer present.

### images - List images
```
docker images
```

This provides a list of available images and their sizes.

### rmi - Remove Images
```
docker rmi
```

Used to remove images that are no longer in use. 

You must ensure that no containers are running off of that image before attempting to remove the image. You must stop and delete all dependent containers to be able to delete an image.

### pull - Download an Image
```
docker pull nginx
```

This command pulls an image, but does not run the container.

### exec - Execute a Command
```
docker exec
```

Used to execute a command on a docker container


## Docker run

### Run -tag

```
docker run redis:4.0
```

This tag can be used if you want to run a specific version of the container you're running.

If you don't specify any tag as in the first command, docker will consider the default tag to be latest(latest version of the software).

At DockerHub, you can look up an image you will find all the supported tags in its description

### Run - STDIN
Suppose you have a simple prompt application that when run asks for your name. On entering your name, prints a welcome message.

If you were to dockerize this application and run it as a container, it wouldn't wait for the prompt and would instead print whatever the application is supposed to print on standard out. By default, the docker container doesn't listen to standard input. It runs in a non-interactive mode.

If you would like to provide your input of your host to the docker container using the `-i` parameter (interactive mode). 

However, the prompt will still remain missing. For this, we use the `-t` option as well (pseudo-terminal).

```
docker run -it kodekloud/simple-prompt-docker
```

## Run - PORT mapping
The underlying host where Docker is installed is called Docker host or Docker engine.

When we run a containerized web application, it runs and we are able to see that the server is running.

How does a user access your application?

Suppose your application is running on port `5000`. What IP do I use to access it from a web browser? There are 2 options

1. Use the IP of the docker container. Every docker container gets an IP assigned by default. This however is an internal IP and is only accessible within the docker host.
2. Use the IP of the docker host, which is `192.168.1.5`. For this to work, you must have mapped the port inside the docker container to a free port on the Docker host. For example, if you want the users to access your application through port `80` on the Docker host, you can map `port 80` on local host to `port 5000` on the docker container using the `-P` parameter in your run command.
```
docker run -p 80:5000 kodekloud/webapp
```

And so, the user can access the application by going to the URL `http://192.168.1.5:80`. All traffic on port 80 on Docker host will get routed to port 5000 inside the docker container.


### RUN - Volume mapping

How is data persisted in a container?

For example, say you were to run a MySQL container. When databases and tables are created, the data files are located in location `/var/lib/mysql` inside the docker container. 

What happens if you were to delete the mysql container and remove it?

As soon as you do that, the container along with all the data inside it gets blown away, meaning all your data is gone.

If you would like to persist data, you would want to map a directory outside the container on the Docker host to a directory inside the container.

In this case, create a directory called `/opt/datadir` and mao that to `/var/lib/mysql` inside the docker container, using the `-v` option and specifying the directory on the docker host, followed by a colon and the directory inside the docker container.

```
docker run -v /opt/datadir:/var/lib/mysql mysql
```

This way, when the docker container runs, it will implicitly mount the external directory to a folder inside the docker container. All your data will now be stored in the external volume at `/opt/data directory` and thus will remain, even if you delete the docker container.

### Inspect Container
The `docker ps` command is good enough to get basic details like their names and IDs, but if you would like to see additional details about a specific container, use the `docker inspect` command and provide the container name or ID.

It return all details of a container in JSON format, such as the state, mounts, configuration data, network settings etc. Remember to use it when you're required to find details on a container.

### Container Logs
How do we see the logs of a container we ran in the background? For example you run a simple web application using the `-d` parameter and it ran the container in a detached mode.

Use the `docker logs` command and specify the container ID or name like this:
```
docker logs blissful_hopp
```