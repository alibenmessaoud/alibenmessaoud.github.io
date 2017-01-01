---
layout: post
title: Docker 100 - Containers
tags: docker docker-series
---

Docker is a tool for creating, deploying, and running apps easily. Docker guarantees that any app will run in the same way on every Docker instance. In this article we will cover the basics of Docker containers, commands and usage.

{% include docker-series.md %} 

To start working with containers, we need to download an Image. Let's start:

## Downloading a Docker Image

We can download image with the `pull` command:

`docker pull [image name]`

For example we can download `nginx` image by using `docker pull nginx`:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
bf5952930446: Pull complete
cb9a6de05e5a: Pull complete
9513ea0afb93: Pull complete
b49ea07d2e93: Pull complete
a5e4a503d449: Pull complete
Digest: sha256:b0ad43f7ee5edbc0effbc14645ae7055e21bc1973aee51507456..
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

## Running a Docker Container

You can set up a basic container with a Bash shell using just one command:

`docker run --name [container name] -i -t [image name] sh`

For example we can run a `nginx` container by using `docker run --name mynginx -i -t nginx sh`. This command we'll start a `nginx` container and open `sh` console:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker run --name mynginx -i -t nginx sh
# ls
bin dev docker-entrypoint.sh home lib64 mnt proc run srv ...
```

Note: The `-i` flag attaches `stdin` and `stdout` and `-t` flag allocates a `tty`.

## Starting a Stopped Container

Sometimes a container will stop, either because you have stopped it, or because its process has ended. In this situation you can run the container again with the container ID or name:

`docker start [container ID or name]`

For our case, we can run `docker start mynginx`

## Connecting to a Running Container

To execute commands inside a running container, we can use the `exec` docker command to connect to a shell session:

`docker exec -it [container ID or name] sh`

For our case, we can run `docker exec -it mynginx sh`

## Disconnecting From a Container

To disconnect or detach from the shell without exiting, use the escape sequence `Ctrl-p` + `Ctrl-q` or you can tape `exit`:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker exec -it mynginx sh
# exit
[node1] (local) root@192.168.0.13 ~
```

## Listing All The Containers

To list the running containers, we use `ps` command:

`docker ps`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker ps
CTAINERID	8243a2f2eb3d
IMAGE 		nginx 
COMMAND		"/docker-entrypoint.…"
CREATED 	7 minutes ago
STATUS 		Up 3 minutes
PORTS 		80/tcp
NAMES		mynginx
```

The `docker ps` command only shows running containers by default. To see all containers, use the `-a` (or `--all`) flag:

`docker ps -a`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker ps
CTAINERID	8243a2f2eb3d			f373a82a2c66
IMAGE 		nginx 					nginx
COMMAND		"/docker-entrypoint..."	"/docker-entrypoint..."
CREATED 	7 minutes ago			3 minutes ago
STATUS 		Up 3 minutes			Exited (0) 2 minutes ago
PORTS 		80/tcp					80/tcp
NAMES		mynginx					bad-nginx
```

The `-q` (or `--quiet `) flag is used to show numeric IDs of the containers.

`docker ps -a -q` list the IDs of the containers:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker ps
f373a82a2c66
8243a2f2eb3d				
```

## Filtering The Running Containers

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`)

`docker ps --filter "name=mynginx"`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker ps --filter "name=mynginx"
CTAINERID	8243a2f2eb3d
IMAGE 		nginx 
COMMAND		"/docker-entrypoint.…"
CREATED 	7 minutes ago
STATUS 		Up 3 minutes
PORTS 		80/tcp
NAMES		mynginx
```

## Stopping a Running Container

To stop a container's process, use the following command:

docker stop [container ID or name]`

For example: `docker stop mynginx`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker stop mynginx
mynginx
```

## Stopping All The Running Containers

To stop all the running containers process, use the following command:

`docker stop $(docker ps -a -q)`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker stop $(docker ps -a -q)
f373a82a2c66
8243a2f2eb3d
```

## Killing a Container

To kill one or more running containers we can use `docker kill` command. The main process inside the container is sent `SIGKILL` signal (default), or the signal that is specified with the `--signal` option.

` docker kill [container ID or name]`

For example: `docker kill mynginx`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker kill mynginx
mynginx
```

## Killing All The Containers

To kill all the running containers we can use `docker kill` command and `docker ps`:

`docker kill $(docker ps -q)`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker kill $(docker ps -a -q)
f373a82a2c66
8243a2f2eb3d
```

## Deleting a Container

A stopped container will continue to exist on your system. We can use the `docker rm` command to tidy up the system and delete all the unwanted containers:

`docker rm [container ID or name]`

For example: `docker rm mynginx`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker rm mynginx
mynginx
```

## Deleting All The Containers

We can use the `docker rm` and `docker ps` commands to delete all the unwanted containers:

`docker rm $(docker ps -a -q)`

```sh
[node1] (local) root@192.168.0.13 ~
$ docker rm $(docker ps -a -q)
f373a82a2c66
8243a2f2eb3d
```