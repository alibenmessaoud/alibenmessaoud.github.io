---
layout: post
title: Docker 104 - Docker Builder Containers
tags: docker docker-series java nodejs spring maven
---

While most people consider `Docker` as an environment to run apps, in reality, `Docker` can be used in the process build as well (i.e. tooling via `Docker`). Yeah! it is used in DevOps with CI/CD tools. In this article, we’ll take a look at creating images to tell containers to build sources instead of running apps.

{% include docker-terminology.md %}

## 1. Use Case

We will allow `Docker` to build sources inside a container by taking control on `Maven` and `NPM`. `Gradle` is an option and we could use it.

## 2. Using `Maven` from `Docker` 

The `Java` project that we will use has the following structure. It is a classic `Spring Boot` project: 

```bash
├── Dockerfile
├── pom.xml
├── README.md
└── src
    ├── main
    │   └── java
    │       └── hello
    │           ├── Application.java
    │           ├── DataController.java
    │           └── Data.java
    └── test
        └── java
            └── hello
                └── DataControllerTests.java

7 directories, 7 files
```

Basically, the normal approach after building the project is to tell `Docker` to copy the built `jar` from the host to the container and run it. I am sure you saw and used the same commands in the past:

```dockerfile
FROM openjdk:8-jdk-alpine
COPY /target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT exec java -jar app.jar
```

This time, our focus will be the `Dockerfile` but we will update it to use `Docker` as a build tool. So we will add some more steps before copying and running the `jar`. We will write the instructions to install `JDK` and `Maven`, copy sources from the host to the container, and then perform `mvn package` or anything else inside the container. Finally, we'll run the resulted `jar`  like in the old version.

The new `Dockerfile` looks like the following:

```dockerfile
FROM maven:3.5.2-jdk-8-alpine
COPY pom.xml /tmp/
COPY src /tmp/src/
WORKDIR /tmp/
RUN mvn package

FROM openjdk:8-jdk-alpine
COPY /target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT exec java -jar app.jar
```

As you can see, this Dockerfile has 2 `FROM` instructions. Each of them represents a stage in a Multi-stage build. You can read more about Multi-stage builds [here](https://docs.docker.com/develop/develop-images/multistage-build/#stop-at-a-specific-build-stage).

Let's build the image using the `Docker` commands: 

`docker build -t java_app .` 

Then, run the container: 

`docker run -p 90:8080 java_app`

```bash
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)

2019-03-21 00:18:51.134  INFO 1 --- [           main] 
hello.Application                        
: Starting Application v0.1.0 on 59725d04bb01 
with PID 1 (/app.jar started by root in /)
2019-03-21 00:18:54.720  INFO 1 --- [           main]
o.s.b.w.embedded.tomcat.TomcatWebServer  
: Tomcat started on port(s): 8080 (http) 
with context path ''
2019-03-21 00:18:54.726  INFO 1 --- [           main] 
hello.Application                        
: Started Application in 4.332 seconds 
(JVM running for 5.068)
```

To test our app, we can see `/greeting` endpoint by using `curl` command or in browser: 

`curl localhost:90/greeting` 

You should see:

`{"content":"Hello, World!"}`

Awesome! All you have to install is `Docker` on your machine you don't need to install JDK and Maven and set the path variables.

But we have a problem. 

> .m2 is fat and it has a lot of dependencies to download!

In other words, the build is slow and we have more dependencies to download. Then, we will run out of space as soon as possible on the host machine. So to make life easier, `Docker` has a great feature called `volumes`. Volumes help to share data between containers and the host. So we know the solution! The idea is to share the `.m2` directory between all the containers for the build step. We can achieve it by applying these instructions: 

- The easy and fast is to add the `--volume` or `-v` option in the build command but I don't recommend it because when we run a container the command will become unreadable:

  `docker run -p 90:8080 java_app -v /Users/myname/.m2:/root/.m2`

- I prefer this option based on `Dockerfile`. You can tell `Maven` to overwrite the `localRepository ` in an inline `settings.xml` file inside the `Dockerfile`: 

  ```xml
  RUN echo \
    "<settings xmlns='https://maven.apache.org/SETTINGS/1.0.0\' \
    xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance' \
    xsi:schemaLocation='https://maven.apache.org/SETTINGS/1.0.0 
    https://maven.apache.org/xsd/settings-1.0.0.xsd'> \
      <localRepository>/Users/myname/.m2/repository</localRepository> \
        <interactiveMode>true</interactiveMode> \
        <usePluginRegistry>false</usePluginRegistry> \
        <offline>false</offline> \
   </settings>" \
  > /usr/share/maven/conf/settings.xml;
  ```

- `docker-compose` is another great option to manage volumes if you have many containers to start at the same time:

  ```yaml
  version: '3'
  services:
    build-and-run-app:
      build:
        context: ./
        dockerfile: Dockerfile
      ports:
        - "90:8080"
      volumes:
        - "/Users/myname/.m2:/root/.m2"
  ```

## 3. Using `Node` from `Docker` 

The `JavaScript` project is very simple and it has the following structure:

```bash
├── app.js
├── Dockerfile
├── package.json
└── views
    ├── css
    │   └── styles.css
    ├── index.html
    └── sharks.html
```

Like in the other section, our focus will be the `Dockerfile`. Generally and after the build step, we tell Docker to copy the built `dist` directory from the host and then run it by using an `nginx` server or anything else.

```dockerfile
FROM nginx:1.16.0-alpine
COPY --from=builder /dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Here we will do some more steps before the run and the classic process in a `Dockerfile`. We will write the instructions to install `Node`, copy sources from the host to the container, and then perform `npm install`. Finally, we'll run the resulted from `dist`.

The targeted `Dockerfile` must be:

```dockerfile
FROM node:10-alpine
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app
COPY package*.json ./
USER node
RUN npm install
COPY --chown=node:node . .
EXPOSE 8080
CMD [ "node", "app.js" ]
```

Let's build the image: 

`docker build -t node_app .` 

Then, run the container: 

`docker run -p 91:8080 node_app`

To test the app, run `curl` on the endpoint: 

`curl localhost:91/greeting` 

And the output is: 

`{"content":"Hello, World!"}`

> Example app listening on port 8080!
> /GET

Awesome!  

But we have a problem. 

> node_modules is fat and a lot of dependencies to download!

The build can get slow if we have more dependencies because `Node` will download them for each build and each container.

Like in the other section, we can share `node_modules`.

According to the doc, you need just set the `NODE_PATH` `env` variable:

`export NODE_PATH='yourdir'/node_modules`

> If the NODE_PATH environment variable is set to a colon-delimited list of absolute paths, then the node will search those paths for modules if they are not found elsewhere. (Note: On Windows, NODE_PATH is delimited by semicolons instead of colons.)

So let's add this line in `Dockerfile`: 

`ENV PATH /root/node_modules/.bin:$PATH`

and in the run we do `-v /Users/myname/node_modules:/root/node_modules` in `docker run`.

Also, in `docker-compose` we make `volumes` for our local `node_modules` folder:

```yaml
version: '3'
services:
  build-and-run-app:
    build:
      context: ./
      dockerfile: Dockerfile
    ports:
      - "91:8080"
    volumes:
      - "/Users/myname/node_modules:/root/node_modules"
```

## 4. Downsides

The main downside of this approach is that it can be hard to maintain the data container. Why? because reading and writing files can be painfully slow on `Windows` and `Mac`. This is a known issue for commands that read and write lots of files, such as `Node.js` and `Java` applications with complex dependencies.

This is because `Docker` runs in a `VM` on `Windows` and `Ma`c. When you do a host volume mount, it has to go through lots of translation to get the folder running on your laptop into the container, somewhat similar to a network file system. This adds a great deal of overhead, which isn’t present when running `Docker` natively on `Linux`.

## 5. Conclusion

In this article, we took a look at creating a general development image that we can use pretty much like our normal command line.