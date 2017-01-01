### What is a domain?

The field for which a system is built. Airport management, insurance sales, coffee shops, orbital flight, you name it.

It's not unusual for an application to span several different domains. For example, an online retail system might be working in the domains of shipping (picking appropriate ways to deliver, depending on items and destination), pricing (including promotions and user-specific pricing by, say, location), and recommendations (calculating related products by purchase history).

### What is a model?

"A useful approximation to the problem at hand." -- Gerry Sussman

An `Employee` class is not a real employee. It *models* a real employee. We know that the model does not capture everything about real employees, and that's not the point of it. It's only meant to capture what we are interested in for the current context.

Different domains may be interested in different ways to model the same thing. For example, the salary department and the human resources department may model employees in different ways.

### What is a domain model?

A model for a domain.

### What is Domain-Driven Design (DDD)?

It is a development approach that deeply values the domain model and connects it to the implementation. DDD was coined and initially developed by Eric Evans.

### What is the blue book that everyone is talking about?

[This one](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)? It is the defining text on Domain-Driven Design, by Eric Evans, the founder of DDD. It comes highly recommended.

### What is a ubiquitous language?

A set of terms used by all people involved in the domain, domain model, implementation, and backends. The idea is to avoid *translation*, because as Eric Evans points out,

> *Translation blunts communication and makes knowledge crunching anemic.*

That is, every time we have to translate concepts between people — "oh, you're using 'user' in these cases where I'm using 'account'" — we lose a direct ability to think clearly about the thing we are building and to let new knowledge flow back and forth between domain and implementation.

Investing in a ubiquitous language pays off in that it makes communication clearer, and allows teams to see more opportunities.

### What is a bounded context?

A division of a larger system that has its own ubiquitous language and domain model. The pricing, shipping, and recommendations aspects of an online retailer would count as separate bounded contexts, as they have significantly different concerns.

As with other DDD concepts, bounded contexts are most valuable when carried through into the implementation.

### How do I go about identifying bounded contexts?

Some common things to look for are:

- The natural boundaries in an organization (within a bounded context, most often you'll find that people collaborate and communicate closely; between bounded contexts the communication is less, and often asynchronous)
- Where the same word is given different meanings (product to pricing is a thing with a price; product to shipping is a thing with a weight and dimensions, etc.)

Generally, good bounded contexts look like products (a pricing strategy product, a shipping calculation product, a product recommendation engine product, etc.) This aligns well with the products-over-projects team structure.

### How isolated should a bounded context be from the rest of my system?

Quite strongly. In general, direct dependencies are best avoided. For example, in .Net separate assemblies would be fairly sensible. In a distributed paradigm, such as SOA or microservices, then finding process boundaries between bounded contexts would be normal.

### How can I communicate between bounded contexts?

Exclusively in terms of their public API. This could involve subscribing to events coming from another bounded context. Or one bounded context could act like a regular client of another, sending commands and queries.

### What are entities? What are value objects?

*Entities* or *reference types* are characterized by having an identity that's not tied to their attribute values. All attributes in an entity can change and it's still "the same" entity. Conversely, two entities might be equivalent in all their attributes, but will still be distinct.

*Value objects* have no separate identity; they are defined solely by their attribute values. Though we are typically talking of objects when referring to value types, native types are actually a good example of value types. It is common to make value types immutable. For example, `String` in many languages is immutable, and every time you want to "change" a string, you derive a new one.

From an event sourcing perspective, both entities and value objects play important roles in the domain, but only entities need be persisted, since only these change.