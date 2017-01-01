---
layout: post
title: Docker 102 - Docker with Java Spring and Maven 
tags: docker java spring docker-series
---

In this article, we'll learn how to create a Docker image of a Spring Boot application, using Dockerfile and Maven, and then run the image we've created.

{% include docker-terminology.md %}

## Generate a Spring App

Let's start with a skeleton project from [Spring Initializr](https://start.spring.io/). Then, select your preferred version of Spring Boot, and add the "Web" dependency. Generate it as a Maven project and you're all set!

The project comes with a simple controller and a single GET mapping:

```java
@RestController
public class DemoController {

  @GetMapping("/greet/{name}")
  public String greeting(@PathVariable String name) {
    return "Hi!! " + name;
  }
}
```

We use the next command from the project root folder to run the app:

`$ mvn spring-boot:run`

Or we can do:

`$ mvn clean package && java -jar target/*.jar` 

As usual, the application will be running on port 8080.

```sh
$ curl http://localhost:8080/greet/ali
Hi!! ali
```

## Dockerize with Dockerfile

As seen in the last articles we can create images with Dockerfile.

Dockerfile is a simple text file a contains a list of commands to instruct Docker to build a proper image for your application.

The basic and most instructions to be used in a Dockerfiles are:

- **FROM** — tells the base image to be used.
  
  You can search for images on [docker-hub](https://hub.docker.com/).

- **WORKDIR** — sets the working directory for the container. 
  
  `WORKDIR` sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` that follow in the Dockerfile.

- **COPY** — tells to copy files from a host directory to the image

- **RUN** — tells to run a command inside the container

- **CMD** — tells to configure a container that will run as an executable and execute the provided command

- **ENTRYPOINT** — It is similar to **CMD** but in an array format

More documentation can be found on the [Dockerfile](https://docs.docker.com/engine/reference/builder/) page.

Our Dockerfile will be as next:

```dockerfile
FROM java:8-jdk-alpine
COPY ./target/demo-0.0.1-SNAPSHOT.jar /usr/app/
WORKDIR /usr/app
RUN sh -c 'touch demo-0.0.1-SNAPSHOT.jar' # optional
ENTRYPOINT ["java","-jar","demo-0.0.1-SNAPSHOT.jar"]
```

Let's take a look at the commands and fully understand them before proceeding:

- **FROM** – tells Docker to use `java` with tag or version `8-jdk-alpine`
- **COPY** - tells Docker to copy our `.jar` file to the build image inside `/usr/app`
- **WORKDIR** - tells Docker to switch the working directory to `/usr/app`
- **RUN** - tells Docker to just "touch" the jar file so that it has its modification time updated because Docker creates all container files in an "unmodified" state by default
- **ENTRYPOINT** - tells Docker to run the Spring Boot app as `java -jar name.jar`

Let's build the image using this Dockerfile:

`$ docker build -t app .`

We gave it a name with the `-t` flag `app `.

Let's check the built image:

`$ docker images`

And finally, let's run the container with this image:

`$ docker run -p 8090:8080 app `

We used `-p` flag to map the port of the host `8090` and the port inside the container `8080`.

Now, we can access the endpoint on `http://localhost:8090/greet/ali`

Our Spring Boot application is successfully running within a Docker container!

## Dockerize with Maven

The process in the last section is manual and need us to run a couple of commands in the terminal. We can create a script to do this in one shot but we'll need to provide a script for each operating system and rename the jar file after each new release. So it would be better if we could make this process a part of a Maven life-cycle so that we can build jar and Docker images in one step which can be useful for CI/CD pipelines.

Hopefully, there are several maven plugins to be used in the `pom.xml` file and make this task easy. Using this approach, there's no need to manually update the Dockerfile, nor run commands in the terminal.

We will use the [fabric8io/docker-maven-plugin](https://github.com/fabric8io/docker-maven-plugin).

Let's add the plugin in the build tag under profiles. By using the profiles, we can keep running the Maven build commands without installing Docker on the machine:

```xml
<profiles>
   <profile>
      <activation>
         <property>
            <name>docker</name>
         </property>
      </activation>
      <build>
         <plugins>
            <plugin>
               <groupId>io.fabric8</groupId>
               <artifactId>docker-maven-plugin</artifactId>
               <version>0.26.0</version>
               <extensions>true</extensions>
               <configuration>
                  <verbose>true</verbose>
                  <images>
                     <image>
                        <name>${project.artifactId}</name>
                        <build>
                           <from>java:8-jdk-alpine</from>
                           <entryPoint>
                              <exec>
                                 <args>java</args>
                                 <args>-jar</args>
                                 <args>/maven/${project.artifactId}-${project.version}.jar</args>
                              </exec>
                           </entryPoint>
                           <assembly>
                              <descriptorRef>artifact</descriptorRef>
                           </assembly>
                        </build>
                     </image>
                  </images>
               </configuration>
               <executions>
                  <execution>
                     <id>build</id>
                     <phase>post-integration-test</phase>
                     <goals>
                        <goal>build</goal>
                     </goals>
                  </execution>
               </executions>
            </plugin>
         </plugins>
      </build>
   </profile>
</profiles>
```

The profile is named `docker` so if we have to build the image using Maven, we should run the command with `-Ddocker` flag.

This part of XML is similar to the content of the Dockerfile but written in XML and some custom variables:

```xml
<name>${project.artifactId}</name>
<build>
 <from>java:8-jdk-alpine</from>
 <entryPoint>
  <exec>
    <args>java</args>
    <args>-jar</args>
    <args>/maven/${project.artifactId}-${project.version}.jar</args>
    </exec>
 </entryPoint>
</build>     
```

The `<name>` tag specifies the name of the image, which is the `artifactId` and will be translated to `demo`.

The `<args>` tags specify how the image should run like `ENTRYPOINT` in a Dockerfile.

Now let's build the image:

`$ mvn clean install -Ddocker `

Let's check the built image:

`$ docker images`

And finally, let's run the container with this image:

`$ docker run -p 8090:8080 demo `

Now, we can access the endpoint on `http://localhost:8090/greet/ali`

## Conclusion

In this article, we saw how to deploy Spring Boot application using Docker using Dockerfile and Maven.