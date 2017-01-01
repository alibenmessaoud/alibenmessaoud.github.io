---
layout: post
title: Docker 101 - Layers
tags: docker docker-series
---

In this article, we will learn how Docker handles Layers to create images and containers.

{% include docker-terminology.md %}

## What are the layers?

While running a container, Docker CLI logs the executed actions one by one and among the logs, many lines are starting with an ID and telling "Pull complete":

```sh
node1] (local) root@192.168.0.18 ~
$ docker pull debian
Using default tag: latest
latest: Pulling from library/debian
952936b044f5: Pull complete 
de09a6a5e5cb: Pull complete 
Digest: sha256:e2164514055ae7bc19...
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest
```

What is this?

According to the documentation, Docker images can consist of multiple layers. So when we pull an image, it pulls one or many layers. In the example above, the image consists of two layers; `952936b044f5` and `de09a6a5e5cb`. So These images are not just one monolithic block but are composed of many layers. The first layer in the image is also called the base layer. 

For example, we can create an image based on `debian` and install `nodejs`:

```dockerfile
FROM debian
RUN apt-get install -y nodejs
```

`docker build -t node_debian_image .`

Once the image is built, you can view all the layers that make up the image with the docker history command.

```sh
docker history node_debian_image 
IMAGE        CREATED  CREATED BY                                SIZE
93d92c6cfdd0 2d ago   /bin/sh -c RUN apt-get install -y nodejs  28MB
b7c5e11b4e6e 5w ago   /bin/sh -c #(nop) ADD file:4f5e3fbdf4…    114MB
```

The result of our image will be two layers: firstly, the base layer from `debian`, and secondly, a layer with a command to install `nodejs`.

```sh
+-----------------------------------------------+
| +-------------------------------------------+ |
| | Layer 1 - RUN apt-get install -y nodejs   | |
| +-------------------------------------------+ |
| +-------------------------------------------+ |
| | Base Layer - FROM debian                  | |
| +-------------------------------------------+ |
| == Image "node_debian_image"  ==              |
+-----------------------------------------------+
```

So, layers are essentially just files generated from running some commands and these commands are nothing but files that will be stacked in this image.

All layers of an image are immutable or read-only which means once created can’t be changed but we can delete it.

When we launch a container from an image, Docker adds a read-write layer to the top of that stack of read-only layers. Docker calls this the Union File System:

`docker run --name node_debian_container node_debian_image `

```sh
+--------------------------------------------------------------+
| +-----------------------------------------+                  |
| | Container Layer                         | +---> Read-Write |
| +-----------------------------------------+                  |
| +-----------------------------------------+                  |
| | Layer 1 - RUN apt-get install -y nodejs | +---> Read-Only  |
| +-----------------------------------------+                  |
| +-----------------------------------------+                  |
| | Base Layer - FROM debian                | +---> Read-Only  |
| +-----------------------------------------+                  |
| == Container "node_debian_container" ==                      |
+--------------------------------------------------------------+
```

By doing this, the same immutable image can be used across various applications just by adding a single writable docker layer. `node_debian_image` can be used to run web apps. 

Let's modify this image and run a web application inside.

The new Dockerfile will use the previous image as a base:

```dockerfile
FROM node_debian_image
COPY server.js /usr/src/app
CMD [ "node", "server.js" ]
```

The example is package in `server.js` file. Let's build the image and see the result.

`docker build -t node_debian_server_image .`

```sh
+---------------------------------------------+
| +-----------------------------------------+ |
| | Layer 2 - COPY server.js /usr/src/app   | |
| |           CMD [ "node", "server.js" ]   | |
| +-----------------------------------------+ |
| +-----------------------------------------+ |
| | Layer 1 - RUN apt-get install -y nodejs | |
| +-----------------------------------------+ |
| +-----------------------------------------+ |
| | Base Layer - FROM debian                | |
| +-----------------------------------------+ |
| == Image "node_debian_server_image"  ==     |
+---------------------------------------------+
```

Let's run and see the result: 

`docker run --name node_debian_server_container node_debian_server_image `

```sh
+----------------------------------------------------------------+
| +-------------------------------------------+                  |
| | Container Layer               server.js   | +---> Read-Write |
| |                                     ^     |                  |
| +-------------------------------------|-----+                  |
|                                       | <-----+ File mapping   |
| +-------------------------------------|-----+                  |
| |                                     |     | +---> Read Only  |
| | Layer 2 - COPY server.js /usr/src/app     |                  |
| |           CMD [ "node", "server.js" ]     |                  |
| +-------------------------------------------+                  |
| +-------------------------------------------+                  |
| | Layer 1 - RUN apt-get install -y nodejs   | +---> Read Only  |
| +-------------------------------------------+                  |
| +-------------------------------------------+                  |
| | Base Layer - FROM debian                  | +---> Read Only  |
| +-------------------------------------------+                  |
| == Image "node_debian_server_container"  ==                    |
+----------------------------------------------------------------+
```

Any time a file is changed, Docker makes a copy of the file from the read-only layers up into the top read-write layer. This leaves the original (read-only) file unchanged.

When a container is deleted, the top read-write layer is lost. This means that any changes made after the container was launched are now gone.

This system helps to save disk space and reducing time to build images while maintaining their integrity. These layers (also called intermediate images) are generated when the commands in the Dockerfile are executed during the build.

## Conclusion

In this article, we discussed Docker layers and how images and containers are created.