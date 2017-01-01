---
layout: post
title: Microservices Patterns with Quarkus
tags: java quarkus Microservices patterns 
---

When using microservices in a project, it’s essential to do it in the right way. Chris Richardson summarized more than 50 patterns about microservices in his book Microservices Patterns and website https://microservices.io. Applying these patterns will help us develop our microservices also with less effort. 

Today, we will see some of them, and we will use Quarkus.

## Project Setup

Let's create a new Playground; in order to create a new project for Quarkus, open [http://code.quarkus.io](http://code.quarkus.io/) and create a new project with these extensions: 

- `quarkus-rest-client`
- `quarkus-smallrye-fault-tolerance`
- `quarkus-smallrye-health`
- `quarkus-smallrye-opentracing`
- `quarkus-smallrye-metrics`

Or download it from https://code.quarkus.io/d?g=io.ms&a=ms-customer&e=rest-client&e=smallrye-health&e=smallrye-opentracing&e=smallrye-metrics&e=smallrye-fault-tolerance&cn=code.quarkus.io!

Also, you can use these commands to configure a new project:

```sh
mvn io.quarkus:quarkus-maven-plugin:1.7.0.Final:create \
    -DprojectGroupId=io.ms \
    -DprojectArtifactId=ms-customer \
    -DclassName="io.ms.customer.CustomerResource" \
    -Dpath="/customer"
cd ms-customer

./mvnw quarkus:add-extension -Dextensions="io.quarkus:quarkus-rest-client"
./mvnw quarkus:add-extension -Dextensions="io.quarkus:quarkus-smallrye-fault-tolerance"
./mvnw quarkus:add-extension -Dextensions="io.quarkus:quarkus-smallrye-health"
./mvnw quarkus:add-extension -Dextensions="io.quarkus:quarkus-smallrye-opentracing"
./mvnw quarkus:add-extension -Dextensions="io.quarkus:quarkus-smallrye-metrics"
```

And finally, you can use IntelliJ IDEA project Wizard to configure a project with these extensions.

If you use `mvn`, run the project with `./mvnw compile quarkus:dev` and you must see:

```sh
Listening for transport dt_socket at address: 5005
__  ____  __  _____   ___  __ ____  ______
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
2020-08-26 10:18:13,806 INFO  [io.quarkus] (Quarkus Main Thread)
ms-customer 1.0-SNAPSHOT on JVM (powered by Quarkus 1.7.0.Final)
started in 1.608s. Listening on: http://0.0.0.0:8080
2020-08-26 10:18:13,820 INFO  [io.quarkus] (Quarkus Main Thread)
Profile dev activated. Live Coding activated.
2020-08-26 10:18:13,820 INFO  [io.quarkus] (Quarkus Main Thread)
Installed features: [cdi, jaeger, rest-client, resteasy, 
smallrye-context-propagation, smallrye-fault-tolerance, 
smallrye-health, smallrye-metrics, smallrye-opentracing]
```

## Externalized Configurations

The primary purpose of the Configuration system is to provide a configuration in the shape of variables for the startup. Also, it must provide a way to modify this configuration from the outside without repackaging the application.

Quarkus use `CDI` and annotations to inject the values or `org.eclipse.microprofile.config.Config` to access the values programmatically in multiple data sources.

The common way is to use `CDI` and the annotations mechanism. We can use `@Inject @ConfigProperty` or just `@ConfigProperty` as follows:

```java
@ConfigProperty(name = "greeting")
String greeting;
```

If the application attempts to inject a missing property, an error is thrown. We can see the result of this case in the next command's output:

```sh
./mvnw compile quarkus:dev
```


```sh
2020-08-26 10:30:57,228 ERROR [io.qua.application] (Quarkus Main Thread) 
Failed to start application (with profile dev):
javax.enterprise.inject.spi.DeploymentException: 
No config value of type [java.lang.String] exists for: greeting 
at io.quarkus.arc.runtime.ConfigRecorder.validateConfigProperties
(ConfigRecorder.java:37)
...
Quarkus application exited with code 1
```

We can use `defaultValue` attribute of `@ConfigProperty` annotation or wrap the value in an `Optional` to avoid this error:

```java
@ConfigProperty(name = "greeting", defaultValue = "hi!")
String greeting;
// Or 
@ConfigProperty(name = "greeting")
Optional<String> mayBeGreeting;
```
> For sake of simplicity, I prefer the first option by providing a default value. Why? 
>
> First,
>
> > TL;DR
> >
> > Java 8's `Optional` was mainly intended for return values from methods, and not for properties of Java classes, as described in [Optional in Java SE 8](http://blog.joda.org/2014/11/optional-in-java-se-8.html)
>
> Second, the use of Optional is the same to `defaultValue` attribute:
> ```java
> ...
> mayBeGreeting.orElse("World")
> ...
> ```
> Third, the default value can lead to use of different values if the config is absent in the class:
> ```java
> // in method 1
> ...
> mayBeGreeting.orElse("value-1")
> ...
> // later in another method
> ...
> mayBeGreeting.orElse("value-2")
> ...
> ```
> Of course, we can use `ConfigProvider` to access the config programmatically:
> ```java
> String greeting = 
>     ConfigProvider
>     .getConfig()
>     .getValue("greeting",  String.class);
> 
> Optional<String> mayBeGreeting = 
>     ConfigProvider
>     .getConfig()
>     .getOptionalValue("greeting", String.class);
> ```

The configuration can be in a container, a cluster, wherever the application is running. To provide the configuration, the easiest way is to create an `applications.properties` file:

```properties
greeting=Quarkus
```

This file will be packaged with application and used at runtime but ...

To overwrite any configuration at runtime, we can set a new configuration as a system property with `-Dproperty.name=value` and/or as an environment variable with `export PROPERTY_NAME=value`.

Configuration from the external are prioritized and override the values in the `applications.properties` file. System properties have more priority than environment variables.

In dev mode, we can use the system properties empowered by Maven: 

```sh
mvn compile quarkus:dev -Dgreeting=Quarkus
```

And at runtime in this way:

```sh
./mvnw clean package -DskipTests
java -Dgreeting=Aloha -jar target/ms-customer-1.0-SNAPSHOT-runner.jar
```

In the case of environment variables, there are three naming conventions for a given property name:

1. Exactly match
2. Replace nonalphanumeric characters to underscore
3. Convert the name to upper case

```sh
./mvnw clean package -DskipTests
export GREETING=hi
java -jar target/ms-customer-1.0-SNAPSHOT-runner.jar
```

In case of Docker, we can pass it easily as an environment variable for Docker:

```sh
docker run -it -p 8080:8080 -e GREETING="Quarkus" .... 
```

**Bonus**: 

`Multivalue` properties are supported—you need to define only the field type as one of `Arrays`, `java.util.List` or `java.util.Set`, depending on your requirements/preference. The delimiter for the property value is a comma `,` and the escape character is the backslash `\`.

The YAML format is also supported for configuring the application. To start using the YAML configuration file, you need to add the `config-yaml` extension:

```sh
./mvnw quarkus:add-extension -Dextensions="config-yaml"
```

In this case, the file is named `application.yaml` or `application.yml`.

## Circuit Breaker

## Health Check

## Distributed Tracing

## API Metrics

