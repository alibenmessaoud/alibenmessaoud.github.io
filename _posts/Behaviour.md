# Behavior Is “Easy”; State Is Hard

# Edson Yanaga

When I was first introduced to object-oriented programming, some of the very first concepts taught were the triple of polymorphism, inheritance, and encapsulation. And to be honest, we spent quite some time trying to understand and code with them. But, at least for me, too much emphasis was given to the first two, and very little to the third and most important of all: encapsulation.

Encapsulation allows us to tame the growing state and complexity that is a constant in the software development field. The idea that we can internalize the state, hide it from other components, and offer only a carefully designed API surface for any state mutation is core to the design and coding of complex information systems.

But, at least in the Java world, we have failed to spread some of the best practices about the construction of well-encapsulated systems. JavaBean properties on anemic classes that simply expose internal state through *getters* and *setters* are common, and with Java Enterprise architectures we seem to have popularized the concept that most—if not all—business logic should be implemented in service classes. Within them we use getters to extract the information, process them to get a result, and then put the result back into our objects with setters.

And when the bugs bite, we dig through log files, use debuggers, and try to figure out what’s happening with our code in production. It’s fairly “easy” to spot bugs caused by behavior issues: pieces of code doing something they’re not supposed to do. On the other hand, when our code seems to be doing the right thing and we still have bugs, it becomes much more complicated. From my experience, the hardest bugs to solve are the ones caused by *inconsistent state*. You’ve reached a state in your system that shouldn’t happen, but there it is—a `NullPointerException` for a property that was never supposed to be `null`, a negative value that should only be positive, and so on.

The odds of finding the steps that led to such an inconsistent state are low. Our classes have surfaces that are too mutable and too easily accessed: any piece of code, anywhere in the system, can mutate our state without any kind of checks or balances.

We sanitize user-provided inputs through validation frameworks, but that “innocent” setter is still there allowing any piece of code to call it. And I won’t even discuss the likelihood of someone using `UPDATE` statements directly on the database to change some columns in database-mapped entities.

How can we solve this problem? *Immutability* is one of the possible answers. If we can guarantee that our objects are immutable, and the state consistency is checked on object creation, we’ll never have an inconsistent state in our system. But we have to take into account that most Java frameworks do not cope very well with immutability, so we should at least aim to minimize mutability. Having properly coded factory methods and builders can also help us to achieve this minimally mutable state.

Therefore, don’t generate your setters automatically. Take time to think about them. Do you really need that setter in your code? And if you decide that you do, perhaps because of some framework requirement, consider using an *anticorruption layer* to protect and validate your internal state after those setter interactions.