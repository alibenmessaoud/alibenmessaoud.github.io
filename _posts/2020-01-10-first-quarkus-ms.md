---
layout: post
title: First Quarkus Microservice
tags: java microservice architecture
---

Supersonic and subatomic! That's the first time I've heard about it in May 2019.

# What is Quarkus?

Quarkus is a powerful enterprise programming stack for building Java applications that deal in serverless, microservices, containers, Kubernetes, or other application types in the hybrid cloud. 

It runs on traditional Java Virtual Machines (JVMs) or natively-compiled executables. 

> Consumption and components are demanded with the smallest possible size/footprint.

Quarkus reduces startup time and memory usage and can make speed and lightweight applications;

The idea of Quarkus is to reduce the dynamic part of Java (reflection, IoC… etc.) at runtime not really needed in a container world. Quarkus proposes to generalize AOT techniques. Indeed, some of the work that usually happens at runtime is moved to the build time. Thus, when the application starts, most of its code has been pre-computed, and the dynamic part of Java won't be executed anymore.

Red Hat has chosen to use standards such as Eclipse MicroProfile, JPA, JAX-RS, Eclipse Vert.x, Netty, etc., to compose this lightweight framework.

# Required Set Up

- Docker
- Java 11 minimum
- Maven

# First microservices

We are going to create a REST API using Quarkus to manage movies.

Quarkus has a plugin for Maven / Gradle to generate a standard project structure with the necessary dependencies.

```sh
$ mvn io.quarkus:quarkus-maven-plugin:1.1.1.Final:create \
    -DprojectGroupId=com.alibenmessaoud.movies \
    -DprojectArtifactId=movies-quarkus-api \
    -DclassName="com.alibenmessaoud.movies.MovieResource" \
    -Dpath="/movies"
$ cd movies-quarkus-api
```

Once the Maven plugin is executed, we will obtain a structure similar to any project in Maven. We can then load the project into an IDE.

Next, let's execute the project, using the command:

```sh
$ mvn quarkus:dev
```

In the browser, access the address `http://localhost:8080`:

```html
Your new Cloud-Native application is ready!
Congratulations, you have created a new Quarkus application.
```

Add the settings below in the `application.properties` file, in the resource folder at the root of the project.

```properties
quarkus.datasource.url = jdbc:postgresql://localhost:5433/movies-quarkus-api
quarkus.datasource.driver = org.postgresql.Driver
quarkus.datasource.username = postgres
quarkus.datasource.password = postgres
quarkus.hibernate-orm.database.generation = drop-and-create
```

Add the dependencies to `pom.xml`.

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-resteasy</artifactId>
</dependency>
<dependency>
  <groupId>io.quarkus</groupId>
  <artifactId>quarkus-resteasy-jsonb</artifactId>
</dependency>
```

And: 

```xml
<dependency>
  <groupId>io.quarkus</groupId>
  <artifactId>quarkus-hibernate-orm-panache</artifactId>
</dependency>
<dependency>
  <groupId>io.quarkus</groupId>
  <artifactId>quarkus-jdbc-postgresql</artifactId>
</dependency>
```

Note: To initialize the database at the beginning of the application, add an `import.sql` file inside the project's resource folder.

I used to run a PostgreSQL database using Docker. For that, it is necessary to have Docker installed and then execute the command listed below:

```sh
$ docker run \
     --name movies-quarkus-api \
     -e "POSTGRES_USER=postgres" \
     -e "POSTGRES_PASSWORD=postgres" \
     -p 5433:5432 \
     -v ~/dev/postgres:/var/lib/postgresql/data \ 
     -d postgres
```

We can use also Docker Compose file:

```yaml
version: '3.5'

services:
  postgres:
    container_name: movies-quarkus-api
    image: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
       - ~/dev/postgres:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped
```

And hit `docker-compose up`!

Movie entity contains the next attributes:

```java
@Entity
public class Movie extends PanacheEntityBase {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Column(name="year")
    private LocalDate year;
...
}
```

We will be using the methods of `PanacheEntityBase` inside the Resource class. We can also create a Repository class and use a Service class.

To access the list of all movies, call the `Movie` class's static methods, which is inherited from the `PanacheEntityBase`.

```java
@Path("/movies/v1")
@Produces(MediaType.APPLICATION_JSON)
public class MovieResource {
    @GET
    public List<Movie> listAll() {
        return Movie.listAll();
    }
...
}
```

Use Postman to add and list your favorite movies.

We can use the `@Transactional` annotation to manage the transaction by committing or rolling back the transaction, if necessary;

```java
...
@POST
@Transactional
public Response create(Movie movie) {
    Movie.persist(movie);
    return Response.ok(movie)
        .status(Response.Status.CREATED)
        .build();

}
..
```

Another way is to implement it in the Repository class. The implementation will be simply implementing `PanacheRepository`:

```java
@ApplicationScoped
public class MovieRepository implements PanacheRepository<Movie> {
    public List<Movie> listAll() {
        return listAll();
    }

...
}
```

The re-implementation in the `MovieResource` class:

```java
@Path("/movies/v2")
@Produces(MediaType.APPLICATION_JSON)
public class MovieResource {
    @Inject
    private MovieRepository movieRepository;

    @GET
    public List<Car> listAll() {
        return movieRepository.listAll();
    }
...
}
```

We can use a NoSQL database too.

We need to configure the application to use a local MongoDB:

```properties
quarkus.mongodb.connection-string=mongodb://localhost:27017
quarkus.mongodb.database=movies
```

We will need to add the following MongoDB with Panache dependency to our project:

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-mongodb-panache</artifactId>
    <version>1.0.1.Final</version>
</dependency>
```

Now,  let's use Docker to start our MongoDB sessions!

```yaml
version: '3.5'
services:
  mongodb_container:
    image: mongo:latest
    ports:
      - 27017:27017
    volumes:
      - ~/dev/mongo:/data/db
volumes:
  mongodb_data_container:
```

And hit `docker-compose up`!

Movie DOCUMENT will contain the same attributes except the id:

```java
public class Movie extends PanacheMongoEntity {
    private String name;
    private LocalDate year;
...
}
```

Then, we create a Repository class. It will extend the `PanacheMongoRepository`:

```java
@ApplicationScoped
public class MovieRepository implements PanacheMongoRepository<Movie> {
 
  public Movie findByName(String name){
    return find("name", name).firstResult();
  }
 
  public List<Movie> findOrderedByName(){
    return findAll(Sort.by("name")).list();
  }
 
}
```

The re-implementation in the `MovieResource` class:

```java
@Path("/movies/v3")
@Produces(MediaType.APPLICATION_JSON)
public class MovieResource {
    private MovieRepository movieRepository;
    
    @Inject
    public MovieResource(MovieRepository knightRepository) {
        this.movieRepository = movieRepository;
    }

    @GET
    public List<Movie> listAll() {
        return movieRepository.listAll();
    }
...
}
```

Use Postman to add and retrieve your favorite movies!

You don't need to re-run because d ev mode use the hot reload feature ;)

# Conclusion

That's it for this tutorial! We created our first Quarkus microservice with a PostgreSQL and MongoDB interaction. You can refine this application and practice your skills now!