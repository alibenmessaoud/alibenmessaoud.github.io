---
layout: post
title: Docker 100 - Images
tags: docker docker-series
---

In this article we will cover the basics of Docker images, commands and usage.

{% include docker-terminology.md %}

## What is a Docker Image?

Docker images are the "source code" for our containers; we use them to build containers. They can have software pre-installed which speeds up deployment. They are portable, and we can use existing images or build our own. In the [previous tutorial]({{ side.baseurl }} {% link _posts/2017-01-03-Docker-100-containers.md %}), we played with `nginx ` image, we pulled the image from the registry and asked the Docker to run a container based on that image. 

```java
[node1] (local) root@192.168.0.13 ~
$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
bf5945930556: Pull complete 
cb9a6de72e5a: Pull complete 
95faea0bxb91: Pull complete 
b49ea07d9583: Pull complete 
a5abe5044749: Pull complete 
Digest: sha256:3f7ee5edbc0e..
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

To see the list of images that are available locally, use the docker images command.

```sh
[node1] (local) root@192.168.0.13 ~
$ docker images
REPOSITORY   TAG     IMAGE ID      CREATED      SIZE
nginx        latest  4bb46517cac3  2 weeks ago  108MB
```

The above gives a list of images that I've pulled from the registry. The `TAG` column refers to a particular snapshot of the image and the `IMAGE ID` column is the corresponding unique identifier for that image.

In other words, if you are a developer and you use `git` everyday, consider an image as a git repository where you can commit changes and have multiple versions. By default, Docker will pull the last image and that's why we see `latest` under `TAG`. Specifying the name of the image to pull `docker pull nginx` is equal to `docker pull nginx:latest`. As we can see, we specify the version after ":". 

For example, you can pull a specific version of `nginx`. 

```sh
[node1] (local) root@192.168.0.13 ~
$ docker pull nginx:1.12.0
$ docker images
REPOSITORY   TAG     IMAGE ID      CREATED      SIZE
nginx        1.12.0  313ec0a602bc  3 weeks ago  107MB
```

To get a new Docker image you can either get it from by exploring the [Docker Hub registery](https://hub.docker.com/explore/), or directly from the command line using `docker search`.

## Docker Container vs. Docker Image?

As in the programming aspect, Image is source code. When source code is compiled and build, it is called an application. Similar to that "when an instance is created for the image", it is called a "container".

**Dockerfile** → (Build) → **Image** → (Run) → **Container**.

## Image Types

We have many types of images. It is a classification and it is not important:

- Base images: OS images, have no parent image like `ubuntu`, `busybox` or `debian`
- Child images: images that build on base images and add additional functionality
- Official images: images that are officially maintained and supported by Docker like `ubuntu`, `busybox` and `hello-world`
- User images: images created and shared by developers built based on base or Official images

## Docker Image Commands

Docker provide a set of commands to work with images:

### Displaying Docker Images

To list the available images on the system:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker images
REPOSITORY   TAG     IMAGE ID      CREATED      SIZE
nginx        1.12.0  313ec0a602bc  3 weeks ago  107MB
```

`-q`  is used to return only the Image ID’s of the images:

```sh
[node1] (local) root@192.168.0.13 ~
$ docker images -q 
31344417fab3
fe1c0c0a602b
ec5ab8b9183c
```

### Pulling Images

Pulls an image or a repository from a registry:

```sh
$ docker pull ImageID
```

### Removing Docker Images

Like the containers, we have a command to remove via an image:

```sh
$ docker rmi ImageID [ImageID_2]
```

### Removing the Unused Images

To remove the unused images:

```sh
$ docker image prune
```

### Removing All the Images 

To remove all images there is a simple command to do that:

```sh
$ docker rmi $(docker images -q)
```

Here we used two command: the command inside the `$()` returns the Image ID’s using the  `-q` flag and the result will be passed then to `docker rmi` to remove all those images.

### Building Images

To build an image from a Dockerfile:

```sh
$ docker build
$ docker build .
$ docker build Dockerfile
$ docker build CustomNameOfDockerfile
$ docker build PathToDockerfile
```

### Loading Inage from a File

Loads an image from a tar archive or streams for receiving or reading input (STDIN):

```sh
$ docker image load
```

### Saving an Image

This command save one or more images to a tar archive (streamed to STDOUT by default):

```sh
$ docker image save
```

### Tag an Image

Creates a tag TARGET_IMAGE that refers to SOURCE_IMAGE:

```sh
$ docker image tag
```

### Pushing an Image

This command pushes an image or a repository to a registry:

```sh
$ docker image push
```

We will be using these commands in the next sections with more options and details.

## Creating and Running First Image

- Step 1 - create a directory for a static website and add `index.html` file
  ```html
  <!doctype html>
  <html>
    <head>
      <title>This is the title!</title>
    </head>
    <body>
        Hi from Docker!
    </body>
  </html>
  ```

- Step 2 - Create a file called Dockerfile
  Place the following contents into the `Dockerfile`:
  
  ```docke
  FROM nginx
  COPY . /usr/share/nginx/html
  ```
  These lines of code represent the image we're going to use along: The `FROM` command helps to select the base image to use for the new image to create. The next command will copy the contents of the current directory into the container.
  
- Step 3 - Build the Docker Image for the HTML Server

  Run the following command: `docker build -t html-server-image:v1 .`

  You can confirm that this has worked by running the command: `docker images`

- Step 4 - Run the Docker Container

  Run the following command to run the HTML container server: `docker run -d -p 80:80 html-server-image:v1`

- Step 5 - Test the Port with cURL

  Run the following command to ensure the server is running: `curl localhost:80`

  You can also view it in the browser now by going to `localhost:80` and you should see your HTML file.

## Conclusion

In this tutorial, we have seen Docker images, and we build and run an Image.