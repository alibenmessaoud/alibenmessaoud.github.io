---
layout: post
title: Portainer for Docker
tags: docker 
---

I use `Docker` every day for running apps, tools, databases, etc.  I can have three of the same apps running side by side using the same port (internally) and without conflict. Moreover, with `Docker Compose`, we can create and start several containers in one shot. This helps a lot and avoid storing commands and configuration of containers in a file.

But, the problem is I used to forget or misuse the command-line options to make Docker work. For example, to see logs or execute commands inside a container. The `Docker` Desktop app works fine, but it needs to be restarted each time to get the last created containers or images.

The closest thing I found to a perfect solution was `Portainer`. It's a web-based and lightweight management UI for Docker that runs as a Docker container. It's easy to get going (Docker for Linux and Docker for Windows are supported). 

In simple words, `Portainer` helps to manage `Docker` containers, images, volumes, networks, etc. For example, we can quickly identify volumes no longer associated with a running container, and if you use volumes everywhere, I am sure that you will find a lot of abandoned volumes just sitting there.

To give it a try, use these commands, and it will provide you with access to the interface on port `9000`:

```sh
$ docker volume create portainer_data
$ docker run -d 
             -p 8000:8000 
             -p 9000:9000 
             --name=portainer 
             --restart=always 
             -v /var/run/docker.sock:/var/run/docker.sock 
             -v portainer_data:/data portainer/portainer-ce
```

`Portainer` directly works with `Docker`, and there is no configuration to change or add. The only thing to do is `localhost:9000/`!

Again, the command is too long and needs to be kept in a file for later reuse, but there's a more straightforward solution for that with a `docker-compose` file!

```yaml
version: '3.8'
services:
  portainer:
    container_name: portainer
    image: portainer/portainer-ce
    restart: always
    command: -H unix:///var/run/docker.sock
    ports:
      - 9000:9000
      - 8000:8000
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - portainer_data:/data
volumes:
  portainer_data:
```

The file reproduce what we have used in the `Docker` `run` command. Save this content under a file called `portainer`, and you can get the server up with this simple command:

```sh
$ docker-compose -f portainer up -d
```

To shut down the server, use:

```sh
$ docker-compose -f portainer down
```

Also, with the provided `volume`, we will not lose any configuration such as the admin account.

Hope you will find this useful!