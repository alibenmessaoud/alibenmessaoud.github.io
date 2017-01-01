---
layout: post
title: Java, RAM and Docker
tags: java docker ram cpu
---

When we run a Java application in physical servers, we can specify the Java heap size. When doing containers, we must consider the same practice. But. By default, in Docker, containers have access to the full CPU and RAM of the host. You might be wondering how to configure RAM and Memory in the container’s world? Are there any best practices? Yes, we must do it because leaving them to run with these default settings may lead to performance bottlenecks. In this tutorial, we will learn how to limit the RAM and CPU usage of Docker containers.

## The Check

Verify whether the used system supports Docker limitations options.

```sh
sudo docker info
...
WARNING: No swap limit support
...
```

If the command returns `WARNING: No swap limit support`, limiting resources has not been enabled by default, and it needs to be activated.

## Limit Docker Container Memory Access

There are several ways to define RAM limitations for Docker containers:

- Defining the maximum amount of memory a container can use
- Defining the amount of memory a Docker container can swap to disk
- Setting the soft limit for the amount of memory assigned to a container

To set the maximum amount of memory for a container, add the `--memory` or `-m` option to the docker run command. It cannot be surpassed: 

```sh
sudo docker run -it --memory=”value” image_name
# value is positive integer and unit without space
# unit = b, k, m, or g (bytes, kilobytes, megabytes, gigabytes)
```

To set the swap to disk memory for a container, add the `--memory-swap` option to the docker run command. This value must be the double of the memory:

```sh
sudo docker run -it --memory=”value” \
                    --memory-swap="valuex2" \
                    image_name
```

To set a soft limit that warns when the container reaches the end of its assigned memory but doesn’t stop any of its services, add the `--memory-reservation` option to the docker run command:

```sh
sudo docker run -it --memory=”value” \
                    --memory-reservation=”value2 < value” \
                    image_name 
```

## Limit Docker Container CPU Usage

There are several ways to define CPU resources for Docker containers:

- Defining the CPUs a container can use
- Defining the relative share of cpu a container can receive after cpu contention 

To set the cpu for a container, add the `--cpus` option to the docker run command:

```sh
sudo docker run -it --cpus=”value” image_name
# value is positive integer
```

 To set the share of cpu for a container, add the `--cpus-shares` option to the docker run command:

```sh
sudo docker run -it --cpus-shares=”value” image_name
```

## What about Java?

Let's run a sample container:

```sh
$ docker run -m 100MB openjdk:8u131 java -XshowSettings:vm -version
VM settings:
    Max. Heap Size (Estimated): 6.98G
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "1.8.0_131"
OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-2-b11)
OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)
```

So the container has 100MB available memory, and the max heap size was set to 6.98G (the area of memory used to store objects instantiated by applications running on the JVM). This tells us that the JVM has no clue that it runs in a container with 100MB available memory.

To fix this, there are many options, and it depends on the version of Java.

Make sure to pass these JVM arguments.

- `-XX:+UnlockExperimentalVMOptions` and

- `-XX:+UseCGroupMemoryLimitForHeap’` 

With these options, JVM will derive the heap size value from the container’s memory size and not from the host’s memory.

Let's add these options:

```sh
$ docker run -m 100MB openjdk:8u131 java \
             -XX:+UnlockExperimentalVMOptions \
             -XX:+UseCGroupMemoryLimitForHeap \
             -XshowSettings:vm \
             -version
VM settings:
    Max. Heap Size (Estimated): 44.50M
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "1.8.0_131"
OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-2-b11)
OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)
```

As mentioned, we will only use 44.50MB out of 100MB but it’s not good enough.

Indeed, there are 3 different options to specify the maximum Java heap size in containers and make the use of java heap optimal. They are:

- `-XX:MinRAMFraction` and `-XX:MaxRAMFraction`

  supported from only Java 8 update 131 to Java 8 update 190

- `-XX:MinRAMPercentage` and `-XX:MaxRAMPercentage`

  supported from Java 8 update 191 and above

- `-Xmx` 

  supported in all versions of Java

### `-XX:MinRAMFraction` and `-XX:MaxRAMFraction`

These options are used to determine the maximum Java heap size. This name makes us think the options are used to configure minimum/maximum heap size. But it’s not true.

By default, the JVM allocates roughly 25% of the max RAM, because `-XX:MaxRAMFraction` defaults to 4. 

Let’s analyze the use of `-XX:MaxRAMFraction`:

```sh
MaxRAM = 1G
MaxRAMFraction = 2
JVM is allowed to allocate: MaxRAM / MaxRAMFraction = 1G / 2 = 512M
```

Translates to: 

```sh
# MaxRAMFraction=2
➜ docker run -m 1GB openjdk:8u131 java \
             -XX:+UnlockExperimentalVMOptions \
             -XX:+UseCGroupMemoryLimitForHeap \
             -XX:MaxRAMFraction=2 \
             -XshowSettings:vm \
             -version
VM settings:
    Max. Heap Size (Estimated): 455.50M
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "1.8.0_131"
OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-2-b11)
OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)
```

The max heap size is 455.50M and not 512M.

Java application’s heap size will be derived from the container’s memory size (because it is fraction basis).

Let's set the fraction to 1:

```sh
$ docker run -m 1GB openjdk:8u131 java \
             -XX:+UnlockExperimentalVMOptions \
             -XX:+UseCGroupMemoryLimitForHeap \
             -XX:MaxRAMFraction=1 \
             -XshowSettings:vm \
             -version
VM settings:
    Max. Heap Size (Estimated): 910.50M
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "1.8.0_131"
OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-2-b11)
OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)
```

The max heap size is 910.50M and not 1G because other processes might be running in the container or connecting to the container with a shell to troubleshoot or inspect the container.

Fraction takes only integer values and not decimals:

```sh
Improperly specified VM option 'MaxRAMFraction=2.5'
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
```

`-XX:MaxRAMFraction` is hard to work with since it’s a fraction so values must be chosen wisely.

This argument has been deprecated in modern Java versions.

### `-XX:MinRAMPercentage` and `-XX:MaxRAMPercentage`

With Java 10 came better support for the Container environment. If environments are running on older JDK versions, this JVM argument can not be used.

The default value for `-XX:MaxRAMPercentage` is 25%.

```sh
docker run -m 1GB openjdk:10 java \
           -XshowSettings:vm \
           -version
VM settings:
    Max. Heap Size (Estimated): 247.50M
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment (build 10.0.2+13-Debian-2)
OpenJDK 64-Bit Server VM (build 10.0.2+13-Debian-2, mixed mode)
```

Let's play with this option. Based on this setting, JVM allocates Max heap size to be 494.9MB (approximately half of 1GB).

```sh
docker run -m 1GB openjdk:10 java \
		   -XX:MaxRAMPercentage=50 \
		   -XshowSettings:vm \
		   -version
VM settings:
    Max. Heap Size (Estimated): 494.94M
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment (build 10.0.2+13-Debian-2)
OpenJDK 64-Bit Server VM (build 10.0.2+13-Debian-2, mixed mode)
```

Note: `-XX:+UseContainerSupport` is passed by default argument in the JVM. No need to explicitly configure it.

```sh
docker run -m 1GB openjdk:10 java \
           -XX:MinRAMPercentage=50 \
           -XX:MaxRAMPercentage=80 \
           -XshowSettings:vm \
           -version
VM settings:
    Max. Heap Size (Estimated): 792.69M
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment (build 10.0.2+13-Debian-2)
OpenJDK 64-Bit Server VM (build 10.0.2+13-Debian-2, mixed mode)
```

With this configuration, we managed to control our JVM to start at 500MB and then grow to maximum 792.69MB

### `-Xmx`

`-Xmx` is supported in non-container and container environments.

```sh
docker run -m 1GB openjdk:8u131 java \
           -Xmx512m \
           -XshowSettings:vm \
           -version VM \
           
VM settings:
    Max. Heap Size: 512.00M
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "1.8.0_131"
OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-2-b11)
OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)        
```

And Java 10

```sh
docker run -m 1GB openjdk:10 java \
		   -Xmx512m \
		   -XshowSettings:vm \
		   -version
VM settings:
    Max. Heap Size: 512.00M
    Using VM: OpenJDK 64-Bit Server VM

openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment (build 10.0.2+13-Debian-2)
OpenJDK 64-Bit Server VM (build 10.0.2+13-Debian-2, mixed mode
```

With this, we master the heap size and set fine-grained/precision values like 512MB, 256MB. 

## Conclusion

That’s it. Now we know how to set the Heap Size for a Java Container.