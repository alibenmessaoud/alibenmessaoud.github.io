---
layout: post
title: Kubernetes 100 - Introduction
tags: kubernetes kubernetes-series cloud
---

Born in Google and donated to Cloud Native Computing Foundation in 2014, 2017 was the year when Kubernetes conquered the container orchestration space as Docker Swarm and Mesos. In this article, we will see the basics of Kubernetes.

The name “Kubernetes” stems from an ancient Greek word for “helmsman,” someone who steers a ship, like a container ship, which explains the ship wheel logo &#9784;.

> Kubernetes is a system for managing containerized applications across a cluster of nodes

- *Written in Go/ Golang* [*https://github.com/kubernetes/kubernetes*](https://github.com/kubernetes/kubernetes)
- *Version 1.0 released in July 2015*
- *Often shortened to K8s(replace 8 characters between k and s)*

## Kubernetes Architecture

Kubernetes cluster consists of Master and Nodes: Master is the controlling machine and Nodes are where your containerized apps run.

### Master

Master is composed of these components:

- `kube-apiserver` (Brain of the cluster): exposes an API (REST, CLI, UI) to manage the cluster
- Cluster key value store (`etcd`): source of truth of the config for the cluster
- `Kube-controller-manager`: regulates the state of the cluster
- `Kube-scheduler`: watches and assigns workloads to nodes

### Nodes, aka Workers or Minions

- `Kubelet`: agent, registers node with cluster, instantiates pods, reports back to master via `kube-apiserver`
- Container engine, or container runtime: usually Docker, pull images and run containers
- Proxy: maintains network rules, performs connection forwarding
- Pod: smallest element of scheduling, unit of deployment, runs containers
- Service: groups together logical collections of pods and enable network access to a set of pods 
- Label: groups several pods together
- Deployment: provides a declarative syntax to create/update pods, keeping a set of pods running

## Launch Single Node Kubernetes Cluster

### Step 0 - Install `Minikube`

`Minikube` is a tool to run Kubernetes locally. It runs a single node cluster inside a VM.

```sh
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
$ sudo dpkg -i minikube_latest_amd64.deb
$ minikube version
minikube version: v0.32.0
commit: ad1ecbcf0099a46b93b620be65c9870ae52bb314
```

### Step 1 - Start Minikube

Let's start the cluster, by running the `minikube start` command:

```sh
$ minikube start --wait=false
* minikube v1.8.1 on Ubuntu 18.04
* Using the none driver based on user configuration
* Running on localhost (CPUs=2, Memory=2460MB, Disk=145651MB) ...
* OS release is Ubuntu 18.04.4 LTS
* Preparing Kubernetes v1.12.5 on Docker 18.05.0-ce ...
  - kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
* Launching Kubernetes ...
* Enabling addons: default-storageclass, storage-provisioner
* Configuring local host environment ...
* Done! kubectl is now configured to use "minikube"
```

We now have a running cluster. `Minikube` created a VM, and started cluster in that VM.

### Step 2 - Cluster Info

The cluster can be interacted with using the `kubectl` CLI. This is the main approach used for managing Kubernetes and the applications running on top of the cluster.

```sh
$ kubectl cluster-info
* Reconfiguring existing host ...
* Using the running none "minikube" bare metal machine ...
* OS release is Ubuntu 18.04.4 LTS
```

```sh
$ kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   2m53s   v0.32.0
```

If the node is marked as **NotReady** then it is still starting the components.

This command shows all nodes to host applications. Now we have only one node, and we can see that it’s status is ready (it is ready to accept applications for deployment).

### Step 3 - Deploy Containers

With a running Kubernetes cluster, containers can now be deployed.

Using `kubectl run`, it allows containers to be deployed onto the cluster:

```sh
$ kubectl create deployment first-deployment --image=docker-http-server
deployment.apps/first-deployment created
```

The status of the deployment can be discovered via the running Pods:

```sh
$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
first-deployment-666c48b44-wb428   1/1     Running   0          69s
```

Once the container is running it can be exposed via different networking options, depending on requirements. One possible solution is `NodePort`, that provides a dynamic port to a container.

```sh
$ kubectl expose deployment first-deployment --port=80 --type=NodePort
service/first-deployment exposed
```

The command below finds the allocated port and executes a HTTP request.

```sh
$ export PORT=$(kubectl get svc first-deployment -o go-template='{{range.spec.ports}}{{if .nodePort}}{{.nodePort}}{{"\n"}}{{end}}{{end}}')
$ echo "Accessing host01:$PORT"
Accessing host01:31118
$ curl host01:$PORT
<h1>Hello, world!</h1>
```

The result is the container that processed the request.

## Conclusion

In this article, we had a quick look at some Kubernetes basics. Kubernetes operates using a very simple model and `minikube` helps to get hands dirty with containers and orchestration. In the next posts, we will share more resources and hands-on tasks.