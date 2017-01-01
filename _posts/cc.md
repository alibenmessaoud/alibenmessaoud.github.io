Learning to write clean code is hard work. It is not about some knowledge of principles and patterns to memorize. It needs practice, more practice and failure to see results until it becomes in your blood. Reading code is the first step to think about what is right about that code and what's wrong with it.

###  Clean Code

Bad code: the result of we'd go back and clean it up later. **Later equals never.**

There's no good code!

Attitude: managers priority is schedule and then code. It's your job to defend the code as manager is defending its schedules.

The Primal Conundrum: respect deadlines = keep the code as clean as possible at all times.

Arts: Clean code is a lot like painting a picture.

Clean code: pleasing to read, exhibits close attention to detail, easy to enhance it, has tests, small, simple.

Runs all tests;

No duplication;

Minimizes the number of entities (classes, methods);

Meaningful names;

Minimal dependencies;

Straightforward logic, no hidden bugs;

### Naming

Name things like you name a child or a pet.

Never change names until you find better ones.

Describe a context: why it exists, what it does, and how it is used.

Avoid disinformation *(`personList` if it is not a `List`)*

Avoid similarly shaped names for dissimilar concepts.

Don't name things to satisfy a compiler or interpreter.

Avoid meaningless distinctions *(ex. `ProductInfo` vs `ProductData`)*.

Use pronounceable names; no encoding or abbreviation.

Avoid Manager, Processor, Data, or Info.

The length of a name, not long and not short.

Avoid mental mapping.

Methods should have verb names.

Accessors should be prefixed with `get`.

Mutators should be prefixed with `set`.

Predicates should be prefixed with `is`.

Don't be cute.

Pick and stick with one word per abstract concept (`get`, `retrieve`, `find`).

The author is responsible for making his code clear.

Use technical names from the solution domain, or else business names from the problem domain.

Place names in context by level classes, functions, or namespaces.

Add prefixes as a last resort.

Needs shared cultural background.

### Functions

Keep very small (< ~10 lines).

Function should be ordered, output of this is the input of the other, and so on.

Extract conditionals to functions (ex. `isLoading()`).

Do one single thing.

The "thing" function should include *one level of abstraction* as its name.

Same level of abstraction as the function name is telling (`getUser` and `parseInt(45)`).

Code to read as narrative text. Each story level leads to the next.

Long and descriptive function names are acceptable than a long comment.

Keep function arguments between 0 and 3.

Think hard about using three arguments.

Reduce the number of arguments by grouping them in a concept.

Keep functions pure.

Don't use flag arguments.

Reduce the number of arguments.

Be careful of dyads (2 arguments) (ex. `something(String, String)`).

Don't manipulate arguments of a function.

Functions should either do something or answer something, but not both.

Prefer exceptions over returning error codes. 

Exceptions allow error handling to be separated cleanly from the "happy path".

Don't repeat yourself.

Refactor and break later. 

Systems are stories to be told.

Extract Try/Catch Blocks: try { method() } catch {log()}.

Create new exceptions are derivatives of the exception class.

### Comments

Sign of lack of ability to express ourselves in code.

Hard to maintain, think hard before you add some.

Inaccurate comments are far worse than no comments.

Legal Comments, Informative Comments (context), Explanation of Intent (decision), Clarification, Warning, TODO, Amplification.

Code smell.

### Formatting

Stick to a common set of style rules.

Keep files between 200 and 500 lines.

Group vertically by concepts.

Vertical distance between concepts.

Declare variables as close to their usage as possible.

Short functions should have all variables declared at top.

Declare instance variables at the top of classes.

Caller functions should be above their callees.

Low-level functions should come last.

Keep lines short (~120 characters).

Use horizontal whitespace to associate strongly related things.

### Objects vs. Data Structures

Data Abstraction: Tell don’t ask.

Abstraction is better than a concretion.

Hiding implementation is about abstraction.

Abstraction allows manipulation of the data, without having to know it's implementation.

Think about the best way to represent the data an object contains.

Don't blindly add getters and setters.

Objects hide their data behind abstractions and expose functions that operate on that data.

Data Structures expose their data and have no meaningful functions.

Data structure better for PP not for OOP.

A module should not know about the innards of the objects it manipulates (LoD).

Law of Demeter doesn't apply to Data Structures.

Train Wrecks: chaining methods can cause problems, avoid.

Hybrid structures that are half object and half data structure are the worst of both worlds. avoid;

Chaining methods can tell structure, avoid;

Objects expose behavior and hide their data. Easy to add new kinds of objects without changing behaviors. Hard to add new behaviors without changing objects.

Data Structures expose data and have no significant behavior. Easy to add new behaviors to existing data structures. Hard to add new data structures to existing functions.

Need flexibility to add new data types. Also flexibility to add new behaviors.

### Error Handling

Keep error handling separate from logic.

Use Exceptions Rather than Return Codes: much cleaner than if else and codes.

Try-Catch-Finally to create good tests and practice TDD, method empty, test throw
exception, write try-catch block, refactor.

Use Unchecked Exceptions: The price of checked exceptions is an OCP, throwing it will
change signature et rebuild all levels, bad, be useful if you are writing a critical library.

Stack traces can't tell you the intent of the failed operation.

Pass helpful error messages with your exceptions.

Wrap 3rd-party APIs - makes it easier to switch and aren't tied to a vendor's API choices.

The Special Case Pattern.

Provide Context with Exceptions;

Define Exception Classes in Terms of a Caller’s Needs: answer “how they are caught?” to
create them;

Don’t Return Null;

Don’t Pass Null;

Treat error handling as a separate concern.

### Boundaries

Using Third-Party Code: keep its objects inside the concerned class, or close family of
classes, where it is used.

Avoid returning it from, or accepting it as an argument to, public APIs;

Exploring and Learning Boundaries;

### Unit Tests

The Three Laws of TDD: write failing test, write no more test, write production code.

Keeping Tests Clean: Test code is just as important as production code.

Clean Tests: Three things keep tests clean: *Readability, readability, and readability*.

Single Concept per Test.

A Dual Standard for running in test or prod environment.

One Assert per Test.

F.I.R.S.T: Fast, Independent, Repeatable, Self-Validating, Timely.

### Classes

Classes Should Be Small; SRP: one, and only one reason to change; Cohesion.

Organizing for Change; OCP: open for extension but closed for modification.

Organizing for Change;

Isolating from Change: A client class depending upon concrete details is at risk when those
details change.

We can introduce interfaces and abstract classes to help isolate the impact of those details.

### Systems:

Separate Constructing a System from Using It: construction is a very different process from
use;

Dependency Injection A powerful mechanism for separating construction from use is Dependency Injection (DI);

Scaling Up; Cross-Cutting Concerns: AOP;

Java Proxies: wrapping method calls in individual objects or classes but it is complex; AOP;

### Emergence:

According to Kent, a design is “simple” if it follows these rules: Runs all the tests, Contains
no duplication, Expresses the intent of the programmer, Minimizes the number of classes
and methods;

### Concurrency:

Concurrency Defense Principles: SRP, Limit the Scope of Data, Use Copies of Data to read
only, Threads Should Be as Independent as Possible;

Know: Thread-Safe Collections in java.util.concurrent;

Know Your Execution Model;

### Successive Refinement

### JUnit Internals

### Refactoring SerialDate

### Smells and Heuristics