---
layout: post
title: CloudEvents Spec
tags: cloud event eda event-driven
---

I've been asking my self the same question for a long time now: what is the best way to define a structure of an event in an event-driven app?

CloudEvents is a specification for describing event data in a common way. In the past we used several formats and conventions to define an envelope of a message and had a lot of problems over time. CloudEvents seeks to ease event declaration and delivery across services!

We will not need to define topics, channels, servers, or subscribers. Just we need to focus on the event and the additional information. The following example shows a CloudEvent serialized as JSON:

```json
{
    "specversion" : "1.0",
    "type" : "com.github.pull_request.opened",
    "source" : "https://github.com/cloudevents/spec/pull",
    "subject" : "123",
    "id" : "A234-1234-1234",
    "time" : "2018-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "text/xml",
    "data" : "<much wow=\"xml\"/>"
}
```
Here the collection of required and optional attributes an event may have:

| Attribute Name                  | Type                          | Note                                                         |
| ------------------------------- | ----------------------------- | ------------------------------------------------------------ |
| id                              | String                        | Required. The ID of the event. A CloudEvent is uniquely identified with its `source` and `id`. |
| source                          | String (URI-reference)        | Required. The source of the event.                           |
| spec<br />version               | String                        | Required. The version of CloudEvents Specification the Cloud Event uses. |
| type                            | String                        | Required. The type of the event.                             |
| data<br />content<br />encoding | String (RFC 2045 Section 6.1) | The encoding of `data` (if the field stores binary data).    |
| data<br />content<br />type     | String (RFC 2046)             | The content type of `data`.                                  |
| schema<br />url                 | String (URI-reference)        | The schema of data.                                          |
| subject                         | String                        | The subject of the event.                                    |
| time                            | String (Timestamp)            | The timestamp when the event happens.                        |
| data                            | N/A                           | The payload of the event.                                    |

Also, developers are allowed to add customs.

Interoperability is the primary driver behind this specification but it could be a source of information leakage. So sensitive information SHOULD NOT be carried or represented in context attributes and domain specific event data SHOULD be encrypted to restrict visibility to trusted parties.

If you are doing serverless, FaaS, or event-driven microservices then this spec is a good solution to follow and I recommend its usage to achieve cloud native apps.
