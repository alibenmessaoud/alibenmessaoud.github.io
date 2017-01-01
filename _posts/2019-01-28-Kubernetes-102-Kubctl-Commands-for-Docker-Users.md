---
layout: post
title: Kubernetes 102 - Kubctl Commands for Docker Users
tags: kubernetes kubernetes-series cloud cli
---

`Kubectl` CLI is a great tool to interact with Kubernetes cluster. In this post, we'll learn the most common `Kubectl` commands by comparing each command to its equivalent in Docker. Indeed if you are familiar with the Docker command line tool, you will see that you are using the same commands as with `kubctl`. 

## docker run

To run a container, we use `run` command. Here we will launch a `nginx` container:

```sh
$ docker run -d --name nginx-app -p 80:80 nginx
d2528dd562291ae72ee032154a76955c189be47d70a33236ff9a490fa169a6db
$ docker ps
CONTAINER ID IMAGE .. NAMES
55c103fa1296 nginx .. nginx-app
```

Pods are the place where we run containers, so in K8s we do:

```sh
$ kubectl create deployment --image=nginx nginx-app
deployment.apps/nginx-app created
```

Then we expose a port through with a service:

```sh
kubectl expose deployment nginx-app --port=80 --type=NodePort --name=nginx-http-service
service/nginx-http exposed
```

## docker ps 

To see the running containers we use `ps` command:

```sh
$ docker ps -a
CONTAINER ID IMAGE .. NAMES
55c103fa1296 nginx .. nginx-app
2e6996792e73 nginx .. k8s_nginx_nginx-app-b8b87
```

In K8s, we do:

```sh
$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
nginx-app-b8b875889-gwcfj   1/1     Running   0          7m13s
```

## docker attach

To attach a running container and see the process' STDIO we use `attach` command. In Docker we ran:

```sh
$ docker attach nginx-app
...
```

In K8s, we do:

```sh
kubectl attach -it k8s_nginx_nginx-app-b8b87
...
```

To detach from the container, we can type the escape sequence Ctrl+P followed by Ctrl+Q.

## docker exec

To execute a command inside in a container, we use `exec` command:

```sh
$ docker exec -it nginx-app sh
# ls lib
init  lsb  systemd  terminfo  udev  x86_64-linux-gnu
```

In K8s, we do:

```sh
$ kubectl.exe exec -it nginx-app-b8b875889-4c6rw -- sh
# ls lib
init  lsb  systemd  terminfo  udev  x86_64-linux-gnu
```

## docker logs 

To follow stdout/stderr of a process that is running we use `logs` command:

```sh
$ docker logs nginx-app
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, 
will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in 
/docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-
listen-on-ipv6-by-default.sh 
...
```

In K8s, we do:

```sh
$ kubectl.exe logs -f nginx-app-b8b875889-4c6rw
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, 
will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in 
/docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-
listen-on-ipv6-by-default.sh 
...
```

## docker stop and docker rm 

To stop and delete a running process, we use `stop` and `rm` commands:

```sh
$ docker stop nginx-app
nginx-app
$ docker rm nginx-app
nginx-app
```

In K8s, we do:

```sh
$ kubectl delete deployment nginx-app
eployment.apps "nginx-app" deleted
$ kubectl get po -l run=nginx-app
No resources found in default namespace.
```

## Conclusion

In this post, we walked through the most used commands in Docker and its equivalent for Kubernetes.