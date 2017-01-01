---
layout: post
title: First Solutions for Docker Hub Rate Limitation
tags: docker docker-limitations kubernetes
---

You have reached your pull rate limit. You may increase the ... This is what you will see by the end of November 1st or maybe later if you don't have a plan. After this date, all image pulls will be put through strict constraints. Anonymous users will be limited to 100 pulls/ 6 hours, and authenticated free users will be limited to 200 pulls/ 6 hours. Docker Pro and Docker Team subscribers will have no restriction as long as they are not abusive and still pay the bills.

## Who's Concerned?

Everyone. Everyone who works with Docker, and the organizations where shared infrastructures and networks are used widely, as the Docker Hub will use the shared IP address.

Particularly, Kubernetes users will be the most concerned by these limitations because they push and pull images during development, often with each container revision. In other words, starting a cluster with some nodes could hit the limit for 3 or 4 developers before even starting the real work. 

## Checking your current rate limit

From now, requests to Docker Hub carry rate limit information in the response headers for each request. To get this information, we need to get authenticated through an HTTP request, generate a token, and emulates a real pull of `test` image.

### Authentication 

Let's request this endpoint `auth.docker.io`, get authenticated and generate a token for the `ratelimitpreview/test` image; 

```sh
$ curl --location --request GET 'https://auth.docker.io/token?\
              service=registry.docker.io&\
              scope=repository:ratelimitpreview/test:pull'
{
    "token": "eyJh3kLf...jupUkIOwIA",
    "access_token": "eyJhbGciOiJS...jupUkIOwIA",
    "expires_in": 300,
    "issued_at": "2020-11-01T06:33:32.689989417Z"
}
```

/!\ Of course, we can use Postman to do these queries;

Now let's take the token in the JSON response, decode it and see what is inside:

```sh
$ jwt eyJhbGciOi...
...
✻ Payload
{
  "access": [
    {
      "type": "repository",
      "name": "ratelimitpreview/test",
      "actions": [
        "pull"
      ],
      "parameters": {
        "pull_limit": "100",
        "pull_limit_interval": "21600"
      }
    }
  ],
  "aud": "registry.docker.io",
  "exp": 1604385512,
  "iat": 1604385212,
  "iss": "auth.docker.io",
  "jti": "QDuKrDyCPek02wM_R-5Z",
  "nbf": 1604384912,
  "sub": ""
}
```

We can observe the attributes `pull_limit` (`100`) and a `pull_limit_interval` (`21600/60/60 = 6`). These values are relative to our use as an anonymous user.

### Get The Remaining Number of Pulls

We need to emulate a real pull of a Docker image but within an `HTTP` request. Yes, it is possible, and the server will return us a manifest file as a response. Again, we will use the image `ratelimitpreview/test`, and bypassing the `token` from the authentication phase. As we said before, this information is in the header of the response, so we need to add `-vs` in the parameter:

```sh
$ curl -vs --location --request GET 'https://registry-1.docker.io\
                /v2/ratelimitpreview/test/manifests/latest' \
                --header 'Authorization: Bearer eyJhbG...'

< HTTP/1.1 200 OK
...
< RateLimit-Limit: 100;w=21600
< RateLimit-Remaining: 81;w=21600
<
{
   "schemaVersion": 1,
   "name": "ratelimitpreview/test",
   ...
}
```

The output tells that our `RateLimit-Limit` is set to 100 pulls/ 6 hours, and under the `RateLimit-Remaining` header value, it still 81 pulls todo in the next 6 hours. Also, performing this same `curl` command multiple times can decrease the `RateLimit-Remaining` value. You can try it!

### What About Authenticated Requests?

To pass as an authenticated user, we need to add `--user "docker_id:password"` in the command to get authenticated and generate a new token. This will increase our rate limit from 100 to 200 pulls.

It is most helpful to prepare yourself and the team for this change because you will hit that limit sooner or later.

## Potential solutions

`Stackoverflow` is the first way to find a solution, but here are some stupid tricks to adopt temporarily so we can continue to work in the next couple of months.

### Google Docker Hub mirror (gcr.io)

Google Cloud is offering a mirror of the Docker Hub at `mirror.gcr.io`, so you need to change the code in your code sources and prefix all the images with this path `mirror.gcr.io/library/`

```sh
$ docker pull mongo

# becomes
$ docker pull mirror.gcr.io/library/mongo
```

Be careful because Google will do the same thing in the future.

### GitHub’s container registry (ghcr.io)

We can use GitHub’s container registry `ghcr.io` to publish our images.

```sh
$ docker pull alibenmessaoud/app-awesome
$ docker pull docker.io/alibenmessaoud/app-awesome

# becomes
$ pull ghcr.io/alibenmessaoud/app-awesome
```

GitHub’s container registry offers unlimited pulls of public images. However, it is not the same product as Docker Hub.

### $5 /month With annual plan 

This is the last simple, straightforward solution by paying $5 /month. With this plan, Docker offers tools you with advanced requirements for 2 parallel builds and unlimited private repositories but without excessive use or abuse of this service.

## Conclusion

Hurry up; changes to the Docker Hub are in place now.