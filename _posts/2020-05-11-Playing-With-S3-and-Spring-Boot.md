---
layout: post
title: AWS - Playing With S3 and Spring Boot
tags: aws spring spring-boot devops
---

This article is about getting a Spring Boot application into production on `AWS`. So to achieve this goal, we will need to have `Docker` installed, and created accounts at `hub.docker.com` and `aws.amazon.com/console`. Moreover, we will use AWS CLI.

## Installing the AWS CLI

AWS CLI is  a powerful tool and provides commands for many and many different AWS services (224 at the time of this writing).

Please follow the [official instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) for your operating system to install version 2 of the AWS CLI on your machine.

After the installation, run `aws configure`. You will be asked to provide 4 parameters:

For “AWS Access Key ID” and “AWS Secret Access Key” you can find them under [your AWS account](https://aws.amazon.com/console/) > account menu > “My Security Credentials” > “Access keys” tab. Once find, create new access key and copy the values into the prompt of the AWS CLI.

For “Region Name”, you can find it in the menu and You can choose between “json” and “yaml” for output format.

```sh
$ aws configure --profile dev
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]:
```

This command will store a configuration file under for the `dev` profile `${user.home}/.aws/credentials`.

To check if everything is fine use this command:

```sh
$  aws ec2 describe-regions
```

Congrats, AWS CLI is now authorized to call the AWS APIs in your name.

## Building an S3 Sample Application

We will use [Spring Cloud for Amazon Web Services](https://cloud.spring.io/spring-cloud-aws/reference/html/##using-amazon-web-services) as wrapped higher-level API operations for AWS that reduce the amount of boilerplate code. So next, let's create the Spring Boot app project.

Spring Cloud doesn't follow Spring Boot versions and it needs to be loaded via `org.springframework.cloud:spring-cloud-dependencies` imported POM or directly via `org.springframework.cloud:spring-cloud-starter-aws` dependency. To be more precise, the `spring-cloud-dependencies` dependency bundles all the cloud starter dependencies to work with cloud libraries unlike the `spring-cloud-starter-aws` which is useful to communicate with AWS services like S3. To make it easier, here's a list of the required dependencies: 

```xml
<!-- option 1 -->
...
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-aws</artifactId>
</dependency>
...

<!-- option 2 -->
...
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-aws</artifactId>
    <version>${spring-cloud-starter-aws.version}</version>
</dependency>
```

Of course, we will need `spring-boot-starter-web` dependency to build Rest controllers.

Just as with any other Spring Boot application, we need to manage and store the app configuration:

```yaml
cloud:
  aws:
    stack:
      auto: false # we don’t rely on AWS CloudFormation service
    credentials:
      profile-name: dev
	  # profilePath: ${user.home}/.aws/credentials
logging:
  level:
    com:
      amazonaws: # fix for known errors
        util:
          EC2MetadataUtils: error
        internal:
          InstanceMetadataServiceResourceFetcher: error
```

The main class to handle communications with S3 is called  `AmazonS3Client`. For example, we can create a `S3Repository ` as follows:

```java
@Component
public class S3Repository {

  private final AmazonS3Client s3Client;

  @Autowired
  public S3Repository(AmazonS3Client s3Client) {
    this.s3Client = s3Client;
  }

  public void create(String bucket) {
    s3Client.createBucket(bucket);
    log.info("Requested create " + bucket);
    s3Client.waiters()
        .bucketExists()
        .run(new WaiterParameters<>(new HeadBucketRequest(bucket)));
    log.info("Bucket " + bucket + " is ready");
  }
    
  ...
  
  public List<String> findAllBuckets() {
    return s3Client.listBuckets()
        .stream()
        .map(Bucket::getName)
        .collect(Collectors.toList());
  }
    
  ...
      
  public FileObject saveFile(String bucket, 
                         String key, 
                         String name, 
                         File file) {
    ObjectMetadata md = new ObjectMetadata();
    md.addUserMetadata("name", name);
    s3Client.putObject(bucket, key, file, md);
      // also s3Client.putObject(bucket, key, <String>, md);
      // also s3Client.putObject(bucket, key, <InputStream>, md);
    log.info("Requested save file " + bucket + "/" + key);

    return FileObject.builder()
        .name(name)
        .key(key)
        .url(s3Client.getUrl(bucket, key))
        .build();
  }
    
  public void makeFilePublic(String bucket, String key) {
    s3Client.setObjectAcl(bucket, key, PublicRead);
    log.info("Requested " + bucket + "/" + key + " as public");
  }

  public void makeFilePrivate(String bucket, String key) {
    s3Client.setObjectAcl(bucket, key, BucketOwnerFullControl);
    log.info("Requested " + bucket + "/" + key + " as private");
  }
    
  public void deleteFile(String bucket, String key) {
    s3Client.deleteObject(bucket, key);
    log.info("Requested delete " + bucket + "/" + key + ");
  }
   
  ...
}
```

There many features and use cases to implement but here are the most used operations. Of course, we can use them inside a Rest controller.

```java
@RestController
@RequestMapping(value = "/s3/bucket")
public class S3BucketApi {

  private S3Repository s3Repository;

  @Autowired
  public S3BucketApi(S3Repository s3Repository) {
    this.s3Repository = s3Repository;
  }

  @GetMapping("/create/{bucket}")
  public ResponseEntity<String> createBucket(@PathVariable String bucket)
      throws URISyntaxException {
    s3Repository.create(bucket);
    return ResponseEntity.created(new URI("/bucket/" + bucket)).build();
  }

  @GetMapping("/delete/{bucket}")
  public ResponseEntity<String> deleteBucket(@PathVariable String bucket) {
    s3Repository.delete(bucket);
    return ResponseEntity.noContent().build();
  }

  @GetMapping("/")
  public ResponseEntity<List<String>> findAllBucket() {
    return ResponseEntity.ok(s3Repository.findAllBuckets());
  }
}
```

In this article, we’ve configured AWS CLI and configured a project to run operations against AWS Simple Storage Service (S3) APIs inside a Spring Boot and the Spring Cloud project.

