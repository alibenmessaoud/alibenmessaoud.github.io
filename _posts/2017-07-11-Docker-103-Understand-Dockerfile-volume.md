---
layout: post
title: Docker 103 - Understand Dockerfile Volume
tags: docker docker-series 
---

In this article, we'll learn how to create a Docker image using `Dockerfile` and volumes, and we will see why we should not declare Docker or host path volumes in a `Dockerfile`.

{% include docker-terminology.md %}

In a `Dockerfile`, the `VOLUME` instruction specifies the volumes with given container-side paths. But it does not allow the image author to define a host path for security reasons and because the path must be defined at runtime as it changes from an OS to another. Hence, this means that the volume will be unnamed.

Let's see how to create a volume with Dockerfile and use it inside a container:

```yaml
FROM alpine
RUN mkdir /directory01
VOLUME /directory01
```

Next, we build the image and tag it:

```bash
$ docker build -t custom-alpin .
```

Then we start the container using the built image and do some changes in a bash session:

```bash
$ docker run -it custom-alpin sh
$ cd directory01
/directory01 $ touch file.txt
/directory01 $ exit
```

Data will be lost after stopping and deleting the container.

If the use of Docker volumes and host path volumes was implemented, think of the following situation:

```
FROM alpine
VOLUME data:/my/data
```

Next, we build the image and tag it:

```
$ docker build -t custom-alpin .
```

Then we start two containers using the built image:

```
$ docker run --name app1 custom-alpin
$ docker run --name app2 custom-alpin
```

Now both `app1` and `app2` would be creating a volume named `data`, and use the same volume. Effectively we would only be able to run a single instance of the image.

## Conclusion

In this article, we saw the use of volumes in `Dockerfile` and why we should avoid declaring Docker volumes and host path volumes. 