---
layout: post
title: Kubernetes 101 - Start Containers Using kubctl
tags: kubernetes kubernetes-series cloud cli
---

We can manage a Deployment by using the Kubernetes CLI called `kubectl`. `kubectl`is based on the API Server to interact with the Kubernetes cluster. In this article, we'll learn the most common `kubectl` commands needed and how to start containers.

## Step 1 - Check The Cluster

After launching a Kubernetes cluster, we wait for the Node to become Ready by checking `kubectl get nodes`:

```sh
$ kubectl get nodes
NAME       STATUS     ROLES    AGE   VERSION
minikube   NotReady   master   9s    v1.17.3
```

When it is Ok, we can go to the next step.

## Step 2 - Kubectl Run

Pods are the place where we can run containers, so to start a pod running `nginx`, we use the `run` command. The `run` command takes some parameters such as the image and creates a deployment file. This deployment will be sent to the master in Kubernetes which launches the Pods and the required container. `Kubectl` run is similar to `docker run` but at a cluster level. 

> If you are familiar with the Docker command line tool, you will see that you are using similar commands as with `kubctl`. 
>
> ```sh
> $ docker run -d --name nginx-app -p 80:80 nginx
> $ kubectl create deployment --image=nginx nginx-app
> ```

The syntax of the command is `kubectl run <name of deployment> <properties>`

The next command launches a deployment called `nginx-app`. It will start a container based on the Docker Image `nginx:latest`:

```sh
$ kubectl run http --image=nginx:latest --replicas=1
deployment.apps/nginx-app created
```

We can then view the status of the deployments:

```sh
$ kubectl get deployments
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
nginx-app   1/1     1            1           108s
```

We can describe the deployment process to see what Kubernetes had done:

 ```sh
$ kubectl describe deployment nginx-app
Name:                   nginx-app
Namespace:              default
CreationTimestamp:      Thu, 24 Jan 2019 19:17:16 +0000
Labels:                 run=nginx-app
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               run=http
Replicas:               1 desired | 1 updated 
                        | 1 total | 1 available 
                        | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  run=nginx-app
  Containers:
   http:
    Image:        nginx:latest
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-app-774bb756bb (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  4m25s  deployment-controller  Scaled up
  replica set nginx-app-774bb756bb to 1
 ```

The description includes how many replicas are available, labels specified and the events associated with the deployment. These events will highlight any problems and errors that might have occurred.

In the next step, we will expose the running service.

## Step 3 - `kubectl` Expose

After creating the deployment, we will create a service to expose the Pod on a port. So we will use `kubectl expose`. This command allows us to define the different parameters of the service and how to expose the deployment.

The idea is ta make the container available on port *80* on the host *8000* binding to the external IP of the host.

```sh
$ kubectl expose deployment nginx-app \ 
        --external-ip="192.168.1.12" \
        --port=8000 \
        --target-port=80
service/nginx-app exposed
```

We will then be able to ping the host and see the result from the HTTP service.

```sh
$ curl http://192.168.1.12:8000
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

## Bonus

With `kubectl run` it's possible to create and expose in one shot command.

```sh
$ kubectl run nginx-one-shot --image=nginx:latest\
                        --replicas=1\
                        --port=80\
                        --hostport=8001
deployment.apps/nginx-one-shot created
```

We should be able to access it using:

```sh
$ curl http://192.168.1.12:8001
<!DOCTYPE html>
...
```

## Step 4 - Scale Containers

Also, we can use `kubectl` to scale the number of replicas of a deployment. Scaling a deployment will request Kubernetes to launch additional Pods. These Pods will then automatically be load balanced using the exposed Service.

```sh
$ kubectl scale --replicas=3 deployment nginx-app
deployment.apps/nginx-app scaled
```

The command `kubectl` scale allows us to adjust the number of Pods running for a particular deployment or replication controller.

Let's list all the pods and see the result:

```sh
$ kubectl get pods
NAME                           READY STATUS  RESTARTS AGE
nginx-app-774bb756bb-2gb6n     1/1   Running 0        2m45s
nginx-app-774bb756bb-vlvt8     1/1   Running 0        34m
nginx-app-774bb756bb-qknz9     1/1   Running 0        2m45s
nginx-one-shot-cd468cb88-46n65 1/1   Running 0        17m5s
```

Once each Pod starts it will be added to the load balancer service. By describing the service we can view the endpoint and the associated Pods which are included.

```sh
$ kubectl describe svc nginx-app
Name:              nginx-app
Namespace:         default
Labels:            run=nginx-app
Annotations:       <none>
Selector:          run=nginx-app
Type:              ClusterIP
IP:                10.96.77.248
External IPs:      192.168.1.12
Port:              <unset>  8000/TCP
TargetPort:        80/TCP
Endpoints:         172.18.0.6:80,172.18.0.8:80,172.18.0.9:80
Session Affinity:  None
Events:            <none>
$
```

## Conclusion

In this article, we created a Kubernetes cluster; then we used it to run deployments, expose services and scale a web application.