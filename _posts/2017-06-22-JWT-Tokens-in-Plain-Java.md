---
layout: post
title: JWT Tokens in Plain Java
tags: java jwt microservices
---

Token authentication is a more modern approach, designed for securing access to microservices and solve problems server-side session IDs can’t. 

## JWT Simply

JWT is a type of token that contains data in a JSON format, afterward encoded as a base 64 string. This doesn’t mean it can be altered though, because additionally to the data, the token also contains a signature. This signature is required to verify the authenticity of the token. Hence, in a man in the middle attack, a third party needs to change the signature as well to fit the payload changes, which is only possible if it knows the secret used to generate the token in the first place.

If you encounter a JWT in the wild, you’ll notice that it’s a long string. Here’s an example of a typical stupid JWT:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
`.eyJzdWIiOiJhbGkiLCJuYW1lIjoiYWxpIn0`
`.Ftg0WZWDT4HsPv0MOiQSGyPxzFXK3Nln5DEVVmHR8CY`

It has 3 parts separated by dots:

1. Header - contains the algorithm for signing the payload
2. Payload - contains the information to transfer
3. Signature - uses for verification

`Header` and `Payload` both are `JSON`. They need to be `base64` encoded and serialized strings. `Signature` is a digest string of the first two parts.

## The JWT flow in detail

Let's see how to create a JWT token, this nice mix of tidy `JSON` objects and strings:

1. Header: `{"alg": "HS256",  "typ": "JWT"}`

   The standard algorithm for signing is `HMAC + SHA256` also called has `HS256`. 

2. Payload: `{"sub": "ali", "name": "ali"}` 

   The data in payload is called claims and tells, at a minimum:

   - Who this person is (the sub claim)
   - What this person is named (the name claim)

3. Signature: `hmacSha256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secet)`

   Because the token is signed with a secret key, you can verify its signature and implicitly trust what is being claimed.

Simply, we can create JWT token in Java, where encryption goes as follows:

```java
String signature = hmacSha256(
    b64e(header) + "." + b64e(payload), 
    secret
);

String jwtToken = b64e(header) 
    				+ "." + b64e(payload) 
    				+ "." + signature;
```

We need to maintain a configurable secret key somewhere but this a subject of another article about secrets management. Here's a simple `HMAC + SHA256` signing method in Java:

```java
private String hmacSha256(String data, String secret) {
  try {
    byte[] hash = secret.getBytes(StandardCharsets.UTF_8);
        
    SecretKeySpec sKey = new SecretKeySpec(hash, "HmacSHA256");
    Mac sha256Hmac = Mac.getInstance("HmacSHA256");
    sha256Hmac.init(sKey);

    byte[] signedBytes = sha256Hmac
            .doFinal(data.getBytes(StandardCharsets.UTF_8));

    return b64e(signedBytes);
  } catch (NoSuchAlgorithmException | InvalidKeyException ex) {
    return null;
  }
}
```

We need to encode the header and payload. For that, we use `Base64` and `URL` encoding:

```java
private static String b64e(byte[] bytes) {
  return Base64.getUrlEncoder()
        	   .withoutPadding()
        	   .encodeToString(bytes);
}
```

Once created, let's see how to verify if the token is valid or not! We need to do the following steps:

1. Parsing, extracting and decoding part 1 and 2
2. Recalculating the signature
3. Comparing the recalculated signature and the received signature

So, let us split the parts using `String.split` method.

```java
String[] parts = token.split("\\.");
```



All three parts are `Base64` `URL` encoded. So use the equivalent decoder:

```java
private static String b64d(String encodedText) {
  return new String(Base64.getUrlDecoder().decode(encodedText));
}
```

We convert the decoded `JSON` `String` to `JSONObject`:

```java
JSONObject payload = new JSONObject(b64d(parts[1]));
```

We can get the values by using the getter methods of the `JSONObject`:

```java
String name = payload.getString("name");
String sub = payload.getString("sub");
```

Now, we can regenerate the signature as explained in an earlier step using the same algorithm. Then, we check this regenerated token if is it matching the signature mentioned in the token?

```java
hmacSha256(parts[0] + "." + parts[1], secret)
	.equals(part[2]);
```

/!\ There are other criteria to validate a token or set it as invalid like the creation date and the source but we need to add this information in the token at creation and use it at the validation. 

> For the sake of clarity, we used a simple string secret to sign the tokens.
>
> The token can be signed with a "private/public key" method; other components then only have to contain the code for checking the signature and know the public key.

There are some downsides to using JWT to store session context including:

- **Base64 encoding isn't encryption**: because base64 is not encryption, everyone in the middle can, at any point, read the content of the JWT. Do not put sensitive information in the token
- **Larger Payload**: the more you put in the JWT, the more you have to send with each and every communication on the wire
- **Token Expiration**: make sure to properly configure the expiration policy of your token so that a stale token can be detected and refreshed

On the other hand, there are a lot of benefits to using JWT:

- **JSON**: it's JSON based and it is awesome
- **Speed and Reliability**: it does not require a third party call which in the context of a distributed system
- **Secure**: it is secure if you are at least using JSON Web Signature and possibly JSON Web Encryption
- **Claims**: it easy for parties to agree on a common set of claims for performing authentication, authorization and more

## Conclusion

Compared to other options, JWTs appear to be the most appropriate option for user authentication in microservices and have more advantages than disadvantages. Besides, JWTs have a significantly smaller footprint compared to other systems. 