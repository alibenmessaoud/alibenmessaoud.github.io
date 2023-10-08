---
layout: post
title: AWS - Deploying Your First Spring Boot App
tags: aws spring spring-boot devops
---

This article is about getting a Spring Boot application into production on `AWS`. So to achieve this goal, we will need to have `Docker` installed, and created accounts at `hub.docker.com` and `aws.amazon.com/console`.

At first, we'll create a simple `Docker` image from a `Hello World` application. It is based on a simple `Spring Boot` application that prints `Hello Docker World` when we visit `/hello` endpoint. We can get fresh `Spring Boot` web project from `https://start.spring.io/` and paste this code:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class Application {
  @RequestMapping("/hello")
  public String hello() {
    return "Hello Docker World";
  }
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }
}
```

Next, we will build a `Docker` image based on `Java 11` using this `Dockerfile`:

```dockerfile
FROM adoptopenjdk/openjdk11:ubi
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

It is very simple, and all we need to run a `Spring Boot` app with no frills: just `Java` and a `JAR` file. 

We can build and the image with the following command: `docker build -t atech/aws-spring-boot-docker .` To check if everything worked out, we can run `docker images | grep atech/aws-spring-boot-docker`.

Let's check if the `Docker` image we just built actually works: `docker run -p 8081:8080 atech/aws-spring-boot-docker`.

Note that without specifying the port mapping, `Docker` won't expose a port on which we can access the application.

Once started, we can open `localhost:8081/hello` in a browser tab and should see `Hello Docker World` text.

To deploy a `Docker` image to `AWS`, `AWS` needs to pull the image from the `Docker registry`. So, let's publish our image to `hub.docker.com` which is `Docker registry`. 

We can publish either from the `UI` or the `CLI`. For both cases, we need a `Docker Hub Account` and being logged in. From the `CLI`, we can login using this command: `docker login registry-1.docker.io`.  Next, we can push the `Docker` image to the registry by using this command: `docker push atech/aws-spring-boot-docker`.

For `AWS`, we will deploy a container via the web based wizard as it is a manual process and cannot be automated. To get started, let's open the [ECS start page](https://console.aws.amazon.com/ecs/home) and click on the `Get started` button.

The link shows a page called `Getting Started` with` Amazon Elastic Container Service (Amazon ECS)` using `Fargate` and with four steps: 

- Step 1: Container and Task: 

  - Container definition section: Click on configure button under the last square called **custom** and enter these information:
    - Container name: `c-aws-spring-boot-docker`
    - Image: `docker.io/atech/aws-spring-boot-docker:latest`

  - Task definition section: Click on edit and update the name `task-aws-spring-boot-docker`
  - We leave everything in the default setting and next

- Step 2: Service

  - We leave everything in the default setting and next

- Step 3: Cluster

  - Update cluster name and set `aws-spring-boot-docker-cluster`
  - We leave everything in the default setting and next

- Step 4: Review 

  - Click on create: the wizard shows these information about preparing the services

    ```sh
    ECS resource creation
        Cluster: aws-spring-boot-docker-cluster
        Task definition: aws-spring-boot-docker:1
        Service
    Additional AWS service integrations
    	Log group: /ecs/aws-spring-boot-docker
    	CloudFormation stack: EC2ContainerService-aws-spring-boot-docker-cluster
    	VPC: vpc- 013e2e84c0bdc79f0
    	Subnet 1
    	Subnet 2
    	Security group
    ```

When all steps are completed, hit the view service button. The summary page shows a whole bunch of information about the status of the service we have just started.

Go to task tab, and click on the task name to see the details. You'll see `ENI Id` link. `ENI` means `Elastic Network Interface` and will lead us to the public `URL` of the container. Scroll to the right where we can see Public `IPv4 address`. Copy that address and add `:8080/hello`. If you followed all the steps as described, I am sure you'll see `Hello Docker World`! Congrats. We've just deployed our first `Docker` container to `AWS`!

To avoid billing problems, don't forget to delete the cluster.

