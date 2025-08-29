---
tags: [java, inheritance, polymorphism, abstract-classes, interfaces, object-methods, cheatsheet]
date: 2025-08-29
topic: Java Inheritance, Polymorphism, and Object Methods
---

# Inheritance

How many parent classes can a Java class inherit from?
What does the final keyword do in a method name?
Can you inherit from classes marked with the final keyword?
Object Methods
When does it make sense to override toString()?
What is getClass() useful for?
What is hashCode() typically used for?
How do equals() and hashCode() relate to each other?
Polymorphism Types
What is static polymorphism?
What is compile-time vs runtime polymorphism?
Abstract Classes and Interfaces
What is the purpose of abstract classes?
What are concrete vs abstract methods?
What are anonymous classes?
How do abstract methods work in derived classes?
What happens when an abstract class implements an interface?
How do interfaces differ from abstract classes?

## Inheritance

### How Many Parent Classes Can a Java Class Inherit From?

- **Only ONE** parent class - Java supports **single inheritance** only.[^1][^2]
- **Multiple inheritance** is NOT supported for classes (but interfaces can extend multiple interfaces).[^2]
- **Reason**: Prevents the "diamond problem" and reduces complexity.

```java
// Valid - single inheritance
class Animal { }
class Dog extends Animal { }  // Dog has ONE parent: Animal

// Invalid - multiple inheritance
// class Dog extends Animal, Mammal { }  // Compilation error!
```


### What Does the `final` Keyword Do in a Method?

- **Prevents method overriding** in subclasses.[^3][^4]
- **Used when** you want to enforce the same implementation across all derived classes.
- **Compiler optimization**: Enables static binding since method cannot change.[^4]

```java
class Parent {
    final void display() {
        System.out.println("Cannot be overridden");
    }
}

class Child extends Parent {
    // void display() { }  // Compilation error!
}
```


### Can You Inherit from Classes Marked with `final`?

- **NO** - Final classes **cannot be extended**.[^5][^4]
- **Examples**: `String`, `Integer`, `Boolean` are final classes.
- **Purpose**: Immutability, security, and preventing unwanted extensions.[^5]

```java
final class MyFinalClass {
    // class implementation
}

// class SubClass extends MyFinalClass { }  // Compilation error!
```


***

## Object Methods

### When Does it Make Sense to Override `toString()`?

- **When you want meaningful string representation** instead of default `ClassName@hashcode`.[^6][^7]
- **Use cases**: Debugging, logging, displaying object state to users.
- **Best practice**: Include key identifying fields, but avoid sensitive data.[^7]

```java
class Student {
    private String name;
    private int id;
    
    @Override
    public String toString() {
        return "Student{name='" + name + "', id=" + id + "}";
    }
}
```


### What is `getClass()` Useful For?

- **Runtime type checking** - determines actual class of an object.[^8][^9]
- **Reflection operations** - getting class metadata, methods, fields.
- **Type-safe operations** in generic programming.[^9]

```java
Object obj = "Hello";
Class<?> clazz = obj.getClass();
System.out.println(clazz.getName());        // java.lang.String
System.out.println(clazz.getSimpleName());  // String

// Compare classes
if (obj1.getClass() == obj2.getClass()) {
    // Same runtime type
}
```


### What is `hashCode()` Typically Used For?

- **Hash-based collections**: `HashMap`, `HashSet`, `Hashtable` use it for **bucketing**.[^10]
- **Performance**: Enables O(1) average lookup time in hash structures.[^10]
- **Must override** when overriding `equals()` to maintain contract.

```java
// String hashCode example
String str = "Hello";
int hash = str.hashCode();  // Used by HashMap for bucket placement
```


### How Do `equals()` and `hashCode()` Relate?

- **Contract**: If `equals()` returns `true`, `hashCode()` **must** return same value.[^11]
- **Not vice versa**: Same hash code doesn't require `equals()` to be true.
- **Always override together** using same fields.[^11]

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    Person person = (Person) obj;
    return Objects.equals(name, person.name) && age == person.age;
}

@Override
public int hashCode() {
    return Objects.hash(name, age);  // Same fields as equals()
}
```


***

## Polymorphism Types

### What is Static Polymorphism?

- **Compile-time polymorphism** achieved through **method overloading**.[^12]
- **Resolved at compilation** based on method signature (parameters).[^12]
- **Also called**: Ad-hoc polymorphism.

```java
class Calculator {
    public int add(int a, int b) { return a + b; }           // Method 1
    public double add(double a, double b) { return a + b; }  // Method 2
    public int add(int a, int b, int c) { return a + b + c; } // Method 3
}
```


### Compile-time vs Runtime Polymorphism

| Aspect          | Compile-time            | Runtime              |
|:----------------|:------------------------|:---------------------|
| **Resolution**  | During compilation      | During execution     |
| **Mechanism**   | Method overloading      | Method overriding    |
| **Binding**     | Static/Early binding    | Dynamic/Late binding |
| **Performance** | Faster (resolved early) | Slightly slower      |
| **Flexibility** | Less flexible           | More flexible        |

```java
// Runtime polymorphism example
Animal animal = new Dog();    // Reference type vs Object type
animal.makeSound();           // Calls Dog's makeSound() at runtime
```


***

## Abstract Classes and Interfaces

### Purpose of Abstract Classes

- **Partial implementation**: Mix of abstract and concrete methods.
- **Common functionality**: Share code among related classes while enforcing certain methods.
- **Template pattern**: Define algorithm structure, let subclasses fill details.

```java
abstract class Shape {
    protected String color;
    
    // Concrete method - shared implementation
    public void setColor(String color) {
        this.color = color;
    }
    
    // Abstract method - must be implemented by subclasses
    abstract double getArea();
}
```


### Concrete vs Abstract Methods

| Method Type  | Definition                | Implementation               | Usage                 |
|:-------------|:--------------------------|:-----------------------------|:----------------------|
| **Concrete** | Complete method with body | Provided in current class    | Ready to use          |
| **Abstract** | Method signature only     | Must be provided by subclass | Forces implementation |

### Anonymous Classes

- **On-the-fly class creation** without explicit class declaration.
- **Common use**: Event handling, callbacks, functional interfaces.
- **Syntax**: `new ClassName() { /* method implementations */ }`

```java
// Anonymous class implementing Runnable
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running in anonymous class");
    }
};
```


### How Abstract Methods Work in Derived Classes

- **Must be implemented** by the first concrete (non-abstract) subclass.
- **Can be left abstract** if subclass is also abstract.
- **Use `@Override`** annotation for clarity.

```java
abstract class Animal {
    abstract void makeSound();
}

class Dog extends Animal {
    @Override
    void makeSound() {        // Must implement
        System.out.println("Woof!");
    }
}
```


### Abstract Class Implementing Interface

- **Can implement some interface methods** and leave others abstract.
- **Subclasses** must implement remaining abstract methods from both class and interface.

```java
interface Drawable {
    void draw();
    void resize();
}

abstract class Shape implements Drawable {
    // Implements one method from interface
    @Override
    public void resize() {
        System.out.println("Resizing shape");
    }
    
    // Leaves draw() abstract - subclasses must implement
}
```


### Interface vs Abstract Class Differences

| Feature                  | Interface                   | Abstract Class          |
|:-------------------------|:----------------------------|:------------------------|
| **Multiple inheritance** | ✅ Yes (implements multiple) | ❌ No (extends one only) |
| **Method types**         | Abstract + default + static | Abstract + concrete     |
| **Fields**               | `public static final` only  | Any access modifier     |
| **Constructor**          | ❌ No                        | ✅ Yes                   |
| **Access modifiers**     | Methods are `public`        | Any access modifier     |
| **Usage**                | Contract/capability         | IS-A relationship       |

***

## Quick Reference Table

| Concept                  | Key Points                      | Example Use Case               |
|:-------------------------|:--------------------------------|:-------------------------------|
| **Single Inheritance**   | One parent class only           | `class Dog extends Animal`     |
| **Final Method**         | Cannot be overridden            | Security, immutable behavior   |
| **Final Class**          | Cannot be extended              | `String`, `Integer` classes    |
| **toString()**           | Custom string representation    | Debugging, logging             |
| **getClass()**           | Runtime type information        | Reflection, type checking      |
| **hashCode()**           | Hash-based collection bucketing | `HashMap`, `HashSet`           |
| **equals() Contract**    | Same logic as hashCode()        | Object equality in collections |
| **Static Polymorphism**  | Method overloading              | Multiple method signatures     |
| **Runtime Polymorphism** | Method overriding               | Dynamic method resolution      |
| **Abstract Class**       | Partial implementation          | Template pattern               |
| **Interface**            | Contract definition             | Multiple capabilities          |

***

## Implementation Hierarchy

```java
// Complete example showing inheritance concepts
interface Drawable {
    void draw();
}

abstract class Shape implements Drawable {
    protected final String type;
    
    public Shape(String type) { this.type = type; }
    
    // Concrete method
    public final String getType() { return type; }
    
    // Abstract method from interface - left for subclasses
    // abstract void draw(); (inherited from Drawable)
    
    // Additional abstract method
    abstract double getArea();
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        super("Circle");
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
    
    @Override
    double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public String toString() {
        return "Circle{radius=" + radius + ", area=" + getArea() + "}";
    }
}
```


***

> [!TIP]
> Always override `equals()` and `hashCode()` together using the same fields to maintain the contract.

> [!WARNING]
> Final classes cannot be mocked easily in unit tests - consider composition over inheritance when using final.

> [!NOTE]
> Use interfaces for capabilities ("can do") and abstract classes for relationships ("is a").

***

\#java \#inheritance \#polymorphism \#final \#abstract-classes \#interfaces \#object-methods \#equals \#hashcode \#bestpractices

⁂

[^1]: https://www.scholarhat.com/tutorial/java/single-inheritance-in-java

[^2]: https://beginnersbook.com/2013/05/java-inheritance-types/

[^3]: https://www.geeksforgeeks.org/java/final-keyword-in-java/

[^4]: https://www.geeksforgeeks.org/java/using-final-with-inheritance-in-java/

[^5]: https://www.scaler.com/topics/final-class-in-java/

[^6]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString

[^7]: https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-override-the-tostring-method

[^8]: https://www.scaler.com/topics/getclass-in-java/

[^9]: https://docs.vultr.com/java/standard-library/java/lang/Object/getClass

[^10]: https://stackoverflow.com/questions/3563847/what-is-the-use-of-hashcode-in-java

[^11]: https://bulldogjob.com/readme/how-not-to-break-your-app-with-hashcode-and-equals

[^12]: https://www.linkedin.com/pulse/understanding-static-dynamic-polymorphism-java-dmitry-nekrasov-8voff

[^13]: https://www.w3schools.com/java/java_inheritance.asp

[^14]: https://www.geeksforgeeks.org/java/inheritance-in-java/

[^15]: https://www.programiz.com/java-programming/inheritance

[^16]: https://www.w3schools.com/java/ref_string_hashcode.asp

