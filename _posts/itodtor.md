In the beginning, Java was an imperative, object-based programming language. Indeed, it still is. Over the years, though, Java has evolved, at each stage becoming more and more a language of declarative expression. *Imperative* is all about the code explicitly telling the computer what to do. *Declarative* is about the code expressing a goal abstracting over the way in which the goal is achieved. Abstraction is at the heart of programming, and so the move from imperative code to declarative code is a natural one.

At the core of declarative expression is the use of higher-order functions, functions that take functions as parameters and/or return functions. This was not an integral part of Java originally, but with Java 8 it moved front and center: Java 8 was a turning point in the evolution of Java, allowing replacement of imperative expression with declarative expression.

An example—trivial but nonetheless indicative of the main issue—is to write a function that returns a `List` containing the squares of the argument `List` to the function. Imperatively, we might write:

```
List<Integer> squareImperative(final List<Integer> datum) {
  var result = new ArrayList<Integer>();
  for (var i = 0; i < datum.size(); i++) {
    result.add(i, datum.get(i) * datum.get(i));
  }
  return result;
}
```

The function creates an abstraction over some low-level code, hiding the details from the code that uses it.

With Java 8 and beyond, we can use streams and express the algorithm in a more declarative way:

```
List<Integer> squareDeclarative(final List<Integer> datum) {
  return datum.stream()
              .map(i -> i * i)
              .collect(Collectors.toList());
}
```

This sets out at a higher level of expression of *what* is to be done; the details of *how* are left to the library implementation. Classic abstraction. True, the implementation is within a function that already abstracts and hides, but which would you rather maintain: the low-level imperative implementation or the high-level declarative implementation?