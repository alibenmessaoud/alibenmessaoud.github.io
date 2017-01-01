---
layout: post
title: The Basics of Elasticsearch Scroll API
tags: java elasticsearch
---

Lately, I have had problems processing many documents from Elasticsearch because the max result window size is 10000 by default. While a search request returns a single page of results, the scroll API can be used to retrieve large numbers of results (or even all results) from a single search request, in much the same way as you would use a cursor on a traditional database. In this article, we will see Scroll API and how to use it in Java and retrieve large number of documents from a single search request.

## Scroll API Under the Hood 

The secret behind the Scroll API is: keep the search context alive! 

In Elasticsearch, each search query creates stores data about search operations and it is called the search context. The more concurrent search operations we have, the more context objects are active at any one time. The Scroll API takes advantage of the search context to provide more accurate data when iterating over documents.

> Pagination is possible by using the attributes 'from', and 'size'  but it has its limit because it can be inaccurate at times. For example, we might occasionally see that documents with the same sort values are not ordered consistently.

To use the Scroll API, we need to specify it in the search request parameters.

```sh
curl -X POST "localhost:9200/my-index-000001/_search?scroll=1m" 
     -H 'Content-Type: application/json' 
     -d'
{
  "size": 100,   # number of results per scroll
  "query": {
    "match": {
      "message": "foo"
    }
  }
}
'
```

This tells Elasticsearch how long it should keep the search context alive, e.g. `?scroll=1m`. 

As a result, Elasticsearch returns the documents and an identifier called `scroll_id`.

```sh
curl -X POST "localhost:9200/_search/scroll?pretty" 
     -H 'Content-Type: application/json' 
     -d'
{
  "scroll" : "1m",                     
  "scroll_id" : "dUDcwakFMNjU1QDXF1ZXJ5QW5B...QlNsd==" 
}
'
```

The idea is to copy that id and pass it to the Scroll API to retrieve the next batch results.

As seen before, the scroll API uses the context of the initial query. So the returned results from a scroll request reflect the state of the index at that time, like a snapshot in time. It ignores any subsequent changes to the matched documents of the context. So this API is not intended for real-time usage but rather for processing large amounts of data.

## Scroll API in Java

To start scrolling, we need to send an initial request with the `scroll` parameter:

```java
var response = client.prepareSearch()
    .setIndices("my-index-000001")
    .setSize(10)
    .setScroll(TimeValue.timeValueMinutes(1))
    .execute()
    .actionGet();

var results = new ArrayList<String>();
response.getHits()
    .forEach(hit -> results.add(hit));
```

To get more results, we need to get the scroll id:

```java
var scrollId = response.getScrollId();
```

And send a new request:

```java
response = client.prepareSearchScroll(scrollId)
    .setScroll(TimeValue.timeValueMinutes(1))
    .execute()
    .actionGet();

response.getHits()
    .forEach(hit -> results.add(hit));
```

We need to do the same thing again to retrieve the next batch of results until there are no more results left. It is also important to use the more recent scroll id in the next request because it may change over time. Let's rewrite the code to get all the documents:

```java
var scrollId = response.getScrollId();

var hasNext = true;

while (hasNext) {
  response = client.prepareSearchScroll(scrollId)
      .setScroll(TimeValue.timeValueMinutes(1))
      .execute()
      .actionGet();
    
  response.getHits()
      .forEach(hit -> results.add(hit));

  hasNext = !newResults.isEmpty();

  scrollId = resp.getScrollId();
}
```

## Java Project Setup for Elasticsearch 

Configure the dependency using maven or Gradle:

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.10.2</version>
</dependency>
```

```groovy
dependencies {
    compile 'org.elasticsearch.client:elasticsearch-rest-high-level-client:7.10.2'
}
```

Then, initialize the client in the code:

```java
var client = new RestHighLevelClient(
    RestClient.builder(
        new HttpHost("localhost", 9200, "http"),
        new HttpHost("localhost", 9201, "http"))
);
```

And don’t forget to close the client:

```java
client.close();
```

## Elasticsearch Setup using Docker and Docker-compose!

```sh
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:7.10.2
# add the '-d' attribute to the run command in foreground
$ docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.10.2
```

```yaml
version: '3.3'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.10.2
    container_name: elasticsearch_devcontainer
    volumes:
      - /var/db/data/elasticsearch:/Users/docker_volumes/elasticsearch_bkp
    ports:
      - 9200:9200
      - 9300:9300
    environment:
      ES_JAVA_OPTS: '-Xms256m -Xmx256m'
      network.bind_host: 0.0.0.0
      network.host: 0.0.0.0
      discovery.type: single-node
```

```sh
$ docker-compose -f docker-compose.yml up -d
```

Navigate to the following URL – `localhost:9200` and check if everything is working!

Docker CLI is not the only choice to operate Elasticsearch in Java. We may consider using other solutions, such as Docker Maven Plugin and Testcontainers or running a cluster manually. 

## Conclusion

In this article, we saw the scroll API basics. Remember the scroll API is designed for data processing, and you may consider the search after and sliced scrolling options for real-time requests.   