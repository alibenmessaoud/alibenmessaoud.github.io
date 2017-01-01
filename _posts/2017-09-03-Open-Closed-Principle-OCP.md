---
layout: post
title: Open/ Closed Principle
tags: best-practice solid
---

In this article, we will see the Open/ Closed Principle (OCP), which is the “O” in the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles.

> The Open/ Closed Principle (OCP) states that “software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.” — Bertrand Meyer, [Object-Oriented Software Construction](https://en.wikipedia.org/wiki/Object-Oriented_Software_Construction)

As a result, it helps to avoid situations where you need to update classes, and publish/ update the libs in all the client projects when you receive a request from the business to change code. Therefore, it is more helpful to extend the entities instead of modifying them. 

The general idea of OCP is excellent but we've seen over the years that inheritance introduces strong coupling if the subclasses depend on implementation details of their parent class. 

For this reason, Robert C. Martin and others revisited the principle and redefined it to the Polymorphic Open/Closed Principle. The new version advises using interfaces instead of super-classes. The interfaces are closed for changes, and you can provide new implementations to extend the functionality of your software.

The principal advantage of this strategy is that an interface introduces an additional level of abstraction which allows loose coupling. The implementations are independent of each other and do not need to share any code.

## Code

### The Bad Way

Let’s say that we’ve got a Rectangle class:

```java
public class Rectangle {
    public double width;
    public double height;
}
```

Our clients need us to build an application to calculate the total area of a set of rectangles:

```java
public class AreaCalculator {
    public double sumArea(Rectangle[] shapes) {
        double area = 0;
        for (var shape : shapes) {
            area = area + shape.Width * shape.Height;
        }
        return area;
    }
}
```

The client is happy and asked to calculate the area of rectangles and also circles. So we did:

```java
public double sumArea(object[] shapes) {
    double area = 0;
    for (var shape : shapes) {
    	if ((shape instanceof Rectangle)) {
           Rectangle rectangle = ((Rectangle)(shape));
           area = (area + (rectangle.width * rectangle.height));
        } else {
           Circle circle = ((Circle)(shape));
           area = (area + (circle.radius * circle.Radius * Math.PI));
        }    
    }
    return area;
} 
```

Later, he asked for triangles. That complicates everything and we are sure that the code will become worse than before.

### The good Way

To solve this puzzle, it would be great to create an interface for everything called "shape".

```java
public interface Shape {
    double sumArea();
}
```

Implementing the `Shape` in the `Rectangle` and `Circle` classes now looks like this:

```java
public class Rectangle extends Shape {    
    public double width;
    public double height;
    public double sumArea() {
        return (this.width * this.height);
    }
}
```

```java
public class Circle extends Shape {
    public double radius;
    public final double sumArea() {
        return this.radius * this.radius * Math.PI;
    }
}
```

The `areaTotal` method which sums all the areas is now much simpler and robust as it can handle any type of `Shape` that we throw at it:

```java
public double areaTotal(Shape[] shapes){
    double area = 0;
    for (var shape: shapes){
        area += shape.sumArea();
    }
    return area;
}
```

In other words, we’ve closed it for modification by opening it up for an extension.

## Conclusion

In this tutorial, we've learned what is OCP by definition and seen bad and good examples.