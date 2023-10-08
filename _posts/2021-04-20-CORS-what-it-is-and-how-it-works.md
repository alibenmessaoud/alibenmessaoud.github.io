---
layout: post
title: CORS - What It Is and How It Works
tags: cors rest frontend backend
---

In modern web application, the frontend and the backend of the applications are separated in several servers, and therefore they have two domains or base URLs. For instance:

- `app.samedomain.com` for the frontend
- `api.samedomain.com` for the backend

Thus when we want to make a request to the `api` domain from the `app` domain, we could encounter this type of error: `Access to fetch at api.samedomain.com from origin app.samedomain.com has been blocked by CORS policy.`

Indeed, the origins are not matching. That's the point where the `CORS` enter in the game!

## So What is This?

It is a browser security feature and called [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

This feature defines which resources provided by a web application are supposed to be accessible from which origin.

> An origin is composed of there parts: protocol, domain and port. or example, `https://example.com` and `https://example.com`. Both have the same domain but different protocols (`http` vs `https`) and ports (`80` vs `443`).

It prevents malicious actors from accessing sensitive data by spoofing a website under a different domain name.

First, for many years a script from one particular site could not access the content of another site, and `cross-origin` requests were forbidden. Later, `cross-origin` requests were allowed, but with new capabilities requiring an explicit permission by the server, and expressed in special headers.

If we want to make the request works for the frontend application, the response should contain a header which is: `Access-Control-Allow-Origin: app.myservice.com`. It means we make the browser understand that the `backend` server knows the `frontend` `origin`, and it's not likely a malicious call.

> We can put `*` instead of the domain but in this case we are removing all security enforced by the `Same-Origin Policy` thus, it is not a good idea.

We also could add a header to tell the allowed methods: `Access-Control-Allow-Methods: GET, HEAD, OPTIONS, POST`.

In the next section, we will see the different options to configure on the client or server side. 

## Types of Cross-Origin Requests

There are two types of cross-origin requests:

1. Safe requests

   `Safe` requests must satisfy the following conditions:

   1. [Safe method](https://fetch.spec.whatwg.org/#cors-safelisted-method): `GET, POST or HEAD`

   2. Safe headers

      – the only allowed custom headers are:

      - `Accept`,
      - `Accept-Language`,
      - `Content-Language`,
      - `Content-Type` with the value `application/x-www-form-urlencoded`, `multipart/form-data` or `text/plain`.

      → The browser sends the `Origin` header with the origin.

      ← For requests without credentials (not sent by default), the server should set:

      - `Access-Control-Allow-Origin` to `*` or same value as `Origin`

      ← For requests with credentials, the server should set:

      - `Access-Control-Allow-Origin` to same value as `Origin`
      - `Access-Control-Allow-Credentials` to `true`

2. All the others.

   Any other request is considered `unsafe`. For instance, a request with `PUT` method or with an `API-Key` `HTTP-header` does not fit the limitations.

   For `unsafe` requests, a preliminary `preflight` request is issued before the requested one:

   - → The browser sends an `OPTIONS` request to the same `URL`, with the headers:
     - `Access-Control-Request-Method` has requested method. 
     - `Access-Control-Request-Headers` lists unsafe requested headers.
   - ← The server should respond with status `200` and the `headers`:
     - `Access-Control-Allow-Methods` with a list of allowed methods,
     - `Access-Control-Allow-Headers` with a list of allowed headers,
     - `Access-Control-Max-Age` with a number of seconds to cache the permissions.
   - Then the actual request is sent, and the previous `safe` scheme is applied.

## How to Add CORS to a Nodejs Express App

Let's take an existing [Node Express](https://expressjs.com/) application and modify it to process cross-origin JavaScript requests.

```sh
mkdir server
cd server
npm init -y
npm install express
touch server.js
```

```js
const express = require('express');
const debug = require("debug")("server");

const app = express();

app.get("/echo", (req, res) => {
  res.send({
    msg: "Hello, World"
  });
});

app.listen(port, () => debug(`Listening on port 4000`));
```

```sh
curl -i localhost:4000/
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 22
Date: Fri, 20 Apr 2021 10:19:39 GMT
Connection: keep-alive

{"msg":"Hello, World"}
```

With Express, we can use middleware functions, and update all HTTP requests coming to the server. These functions can:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

Let's modify the code to return CORS headers and make the API calls work from any browser. 

```js
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept"
  );
  next();
});
```

```sh
curl -i localhost:4000/
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Content-Type: application/json; charset=utf-8
Content-Length: 22
Date: Fri, 20 Apr 2021 10:21:12 GMT
Connection: keep-alive

{"msg":"Hello, World"}
```

Here we can see the headers have been added correctly.

Instead of manually specifying the headers, there is a CORS Express middleware package that can be used to write minimal and safe CORS configuration.

```sh
npm install cors
```

```js
app.use( 
    cors({
    origin: "localhost:4001",
    methods: "GET"
  })
);
```

This code will restrict calls to those the address localhost:4001 and GET method.

I hope this read gave you a good idea about `CORS`, how it came to be, and why it's necessary. 

