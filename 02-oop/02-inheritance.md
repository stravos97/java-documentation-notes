---
tags: [java, inheritance, polymorphism, abstract-classes, interfaces, object-methods, cheatsheet]
date: 2025-08-29
topic: Java Inheritance, Polymorphism, and Object Methods
---

# Inheritance

1. How many parent classes can a Java class inherit from?
2. What does the final keyword do in a method name?
3. Can you inherit from classes marked with the final keyword?
4. Object Methods
5. When does it make sense to override toString()?
6. What is getClass() useful for?
7. What is hashCode() typically used for?
8. How do equals() and hashCode() relate to each other?
9. Polymorphism Types
10. What is static polymorphism?
11. What is compile-time vs runtime polymorphism?
12. Abstract Classes and Interfaces
13. What is the purpose of abstract classes?
14. What are concrete vs abstract methods?
15. What are anonymous classes?
16. How do abstract methods work in derived classes?
17. What happens when an abstract class implements an interface?
18. How do interfaces differ from abstract classes?

### Inheritance

#### How Many Parent Classes Can a Java Class Inherit From?

- **Single Inheritance**: Java allows a class to extend **only one** parent class directly.
- **No Multiple Inheritance**: Classes cannot extend multiple classes. This avoids the "diamond problem," where ambiguity arises if two parent classes have the same method.
- **Multiple Inheritance of Interfaces**: While a class can implement multiple interfaces, it can only extend one class.

```java
class Animal { }
class Mammal { }
class Dog extends Animal { } // Valid
// class MagicDog extends Animal, Mammal { } // Error: Multiple inheritance not allowed

interface Eats { void eat(); }
interface Runs { void run(); }
class Cheetah extends Animal implements Eats, Runs { } // Valid: Implements multiple interfaces
```

> [!TIP]
> Use **interfaces** when you need a class to have multiple "capabilities."
> Use **single inheritance** for "is-a" relationships.

***

#### final Keyword in Methods

- **No Overriding**: A `final` method cannot be overridden in subclasses.
- **Enforces Consistency**: Ensures the method implementation remains unchanged.
- **Used For**: Security, immutability, or when the method’s behavior must not change.

```java
class Parent {
    final void lock() {
        System.out.println("Cannot be changed!");
    }
}

class Child extends Parent {
    // @Override
    // void lock() { System.out.println("Won't compile!"); } // Error
}
```

> [!WARNING]
> **final** methods can reduce flexibility—use only when necessary.

***

#### final Classes

- **Cannot Be Subclassed**: A `final` class cannot be extended.
- **Use Cases**: Security (prevent malicious subclassing), immutability (e.g., `String`), or when the class shouldn’t be changed.
- **Testing Implications**: Final classes are harder to mock in unit tests.

```java
final class ImmutablePerson { }
// class Employee extends ImmutablePerson { } // Error: Cannot subclass final class
```

> [!TIP]
> Prefer **composition** over inheritance when you want to reuse behavior from a final class.

***

### Object Methods

#### Overriding toString()

- **Purpose**: Provides a meaningful string representation for debugging, logging, and user displays.
- **Best Practice**: Include relevant fields, exclude sensitive data.

```java
class Student {
    private String name;
    private int id;

    @Override
    public String toString() {
        return "Student{name='" + name + "', id=" + id + "}";
    }
}

Student s = new Student();
System.out.println(s); // Prints: Student{name='null', id=0}
```

***

#### getClass()

- **Returns Runtime Class**: Reveals the actual class of an object, useful for reflection and runtime type checking.
- **Example Uses**: Logging, type-safe operations, and dynamic method invocation.

```java
Object obj = "Hello";
Class<?> clazz = obj.getClass();
System.out.println(clazz.getName()); // java.lang.String
```

***

#### hashCode() and equals()

- **hashCode()**: Used by hash-based collections (`HashMap`, `HashSet`) for efficient storage and retrieval.
- **equals()**: Determines object equality.
- **Contract**: If two objects are equal (`equals()` returns `true`), their hash codes must be the same.
- **Always Override Together**: Use the same fields in both methods.

```java
class Person {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

> [!WARNING]
> **Breaking the contract** between `equals()` and `hashCode()` can cause hard-to-find bugs in collections.

***

### Polymorphism

#### Static Polymorphism (Method Overloading)

- **Same Name, Different Parameters**: Multiple methods in the same class share a name but differ by parameter types or counts.
- **Resolved at Compile Time**: The compiler chooses the correct method based on the argument types.

```java
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
}
```

> [!NOTE]
> **Static polymorphism** is flexible but limited to one class.

***

#### Dynamic Polymorphism (Method Overriding)

- **Method Overriding**: A subclass provides its own implementation of a method defined in its superclass.
- **Determined at Runtime**: The actual method called depends on the object's runtime type.
- **Uses**: Frameworks, flexible APIs, and template patterns.

```java
class Animal {
    void sound() { System.out.println("Animal makes a sound"); }
}
class Cat extends Animal {
    @Override void sound() { System.out.println("Meow"); }
}
Animal a = new Cat();
a.sound(); // Output: Meow
```

### Polymorphism Decision Flow

Here's how Java determines which method to call in different polymorphic scenarios:

```mermaid
flowchart TD
    START["Method Call: obj.methodName(args)"] --> COMPILE_TIME{Compile Time\nAnalysis}
    
    COMPILE_TIME --> CHECK_OVERLOAD{Multiple methods\nwith same name\nbut different params?}
    
    CHECK_OVERLOAD -->|Yes| OVERLOAD_RESOLVE["Static Polymorphism\n(Method Overloading)"]
    CHECK_OVERLOAD -->|No| SINGLE_METHOD["Single method signature"]
    
    OVERLOAD_RESOLVE --> PARAM_MATCH{Find exact\nparameter match?}
    PARAM_MATCH -->|Yes| EXACT_MATCH["Use exact match"]
    PARAM_MATCH -->|No| WIDENING["Apply widening conversions\nFind best match"]
    WIDENING --> COMPILE_SUCCESS["Compile-time resolution complete"]
    EXACT_MATCH --> COMPILE_SUCCESS
    
    SINGLE_METHOD --> COMPILE_SUCCESS
    COMPILE_SUCCESS --> RUNTIME{"Runtime\nExecution"}
    
    RUNTIME --> INHERITANCE_CHECK{Method overridden\nin subclass?}
    
    INHERITANCE_CHECK -->|Yes| DYNAMIC_RESOLVE["Dynamic Polymorphism\n(Method Overriding)"]
    INHERITANCE_CHECK -->|No| STATIC_CALL["Direct method call"]
    
    DYNAMIC_RESOLVE --> VTABLE["Check Virtual Method Table\n(V-Table)"]
    VTABLE --> ACTUAL_TYPE["Find actual object type\nat runtime"]
    ACTUAL_TYPE --> OVERRIDE_CALL["Call overridden method\nin actual class"]
    
    STATIC_CALL --> DIRECT_EXEC["Execute method directly"]
    OVERRIDE_CALL --> DIRECT_EXEC
    
    style START fill:#e3f2fd
    style OVERLOAD_RESOLVE fill:#fff3e0
    style DYNAMIC_RESOLVE fill:#f3e5f5
    style COMPILE_SUCCESS fill:#d4edda
    style DIRECT_EXEC fill:#c8e6c9
```

**Method Resolution Examples:**

```mermaid
graph LR
    subgraph "Overloading (Compile-Time)"
        OL1["calc.add(5, 3)"] --> OL2["Compiler sees:\nadd(int, int)"]
        OL3["calc.add(5.0, 3.0)"] --> OL4["Compiler sees:\nadd(double, double)"]
        OL5["calc.add(5, 3, 2)"] --> OL6["Compiler sees:\nadd(int, int, int)"]
        
        OL2 --> OL7["Returns: 8 (int)"]
        OL4 --> OL8["Returns: 8.0 (double)"]
        OL6 --> OL9["Returns: 10 (int)"]
    end
    
    subgraph "Overriding (Runtime)"
        OR1["Animal a = new Dog();\na.sound();"] --> OR2["Compile: Checks Animal.sound()\nexists and is accessible"]
        OR2 --> OR3["Runtime: JVM sees actual\nobject is Dog instance"]
        OR3 --> OR4["Calls Dog.sound()\nOutput: 'Woof!'"]
        
        OR5["Animal a = new Cat();\na.sound();"] --> OR6["Compile: Checks Animal.sound()\nexists and is accessible"]
        OR6 --> OR7["Runtime: JVM sees actual\nobject is Cat instance"]
        OR7 --> OR8["Calls Cat.sound()\nOutput: 'Meow!'"]
    end
    
    style OL1 fill:#fff3e0
    style OL3 fill:#fff3e0
    style OL5 fill:#fff3e0
    style OR1 fill:#f3e5f5
    style OR5 fill:#f3e5f5
    style OL7 fill:#d4edda
    style OL8 fill:#d4edda
    style OL9 fill:#d4edda
    style OR4 fill:#d4edda
    style OR8 fill:#d4edda
```

**Polymorphism Type Comparison:**

| Aspect | Static Polymorphism (Overloading) | Dynamic Polymorphism (Overriding) |
|:-------|:---------------------------------|:----------------------------------|
| **Resolution Time** | Compile-time | Runtime |
| **Based On** | Method signature (parameters) | Object's actual type |
| **Inheritance Required** | No (same class) | Yes (inheritance hierarchy) |
| **Performance** | Faster (no runtime lookup) | Slightly slower (v-table lookup) |
| **Flexibility** | Limited to parameter variations | High (different implementations) |
| **Example Keywords** | Multiple methods, same name | `@Override`, `extends` |

***

### Abstract Classes

#### Purpose and Usage

- **Partial Implementation**: An **abstract class** can have both abstract (unimplemented) and concrete methods.
- **Framework Classes**: Useful for providing a common structure while allowing subclasses to fill in details.
- **Cannot Be Instantiated**: You can’t create an object of an abstract class directly.

```java
abstract class Shape {
    abstract double area();
    String getType() { return getClass().getSimpleName(); }
}
class Circle extends Shape {
    double radius;
    Circle(double r) { radius = r; }
    @Override double area() { return Math.PI * radius * radius; }
}
```

***

#### Concrete vs Abstract Methods

| Type         | Definition                | Example                    |
| :----------- | :------------------------ | :------------------------- |
| **Concrete** | Complete method with body | `String getType() { ... }` |
| **Abstract** | Only signature, no body   | `double area();`           |

- **Concrete** methods are inherited as-is.
- **Abstract** methods must be implemented by the first concrete subclass.

***

#### Anonymous Classes

- **Inline Implementation**: Define a class on the spot without a name, usually to implement an interface or extend a class.
- **Common in Event Handling**: E.g., GUI listeners, functional interfaces.

```java
Runnable task = new Runnable() {
    @Override public void run() {
        System.out.println("Running!");
    }
};
new Thread(task).start();
```

***

#### Abstract Classes Implementing Interfaces

- **Partial Implementation**: The abstract class can implement some interface methods, leaving others for subclasses.
- **Flexible Design**: Allows you to share common code while still requiring subclasses to implement specific behavior.

```java
interface Drawable {
    void draw();
    void resize();
}

abstract class Shape implements Drawable {
    @Override public void resize() {
        System.out.println("Resizing shape");
    }
    // draw() is still abstract—subclasses must implement
}

class Circle extends Shape {
    @Override public void draw() { System.out.println("Drawing circle"); }
}
```

***

### Interfaces vs Abstract Classes

| Feature                  | Interface                             | Abstract Class                    |
| :----------------------- | :------------------------------------ | :-------------------------------- |
| **Multiple Inheritance** | Yes (implements multiple)             | No (extends one only)             |
| **Method Types**         | Abstract + default + static (Java 8+) | Abstract + concrete               |
| **Fields**               | Only `public static final`            | Any access modifier               |
| **Constructors**         | No                                    | Yes                               |
| **Usage**                | Define contracts/capabilities         | Provide common structure/behavior |
| **Default Access**       | Methods are `public`                  | Methods can have any access       |

> [!TIP]
> **Use abstract classes** for "is-a" relationships and shared code.
> **Use interfaces** for "can-do" relationships and multiple capabilities.

***

### Quick Reference Table

| Concept                  | Key Points                          | Example Use Case           |
| :----------------------- | :---------------------------------- | :------------------------- |
| **Single Inheritance**   | Extend one class only               | `class Dog extends Animal` |
| **Final Method**         | No overriding in subclasses         | Security, immutability     |
| **Final Class**          | No subclassing                      | `String`, `Integer`        |
| **toString()**           | User-friendly object representation | Debugging, logging         |
| **getClass()**           | Runtime type information            | Reflection, type checking  |
| **hashCode()**           | Hash-based collection efficiency    | `HashMap`, `HashSet`       |
| **equals() Contract**    | Must match `hashCode()` logic       | Collections, equality      |
| **Static Polymorphism**  | Overloading (compile-time)          | Multiple method signatures |
| **Dynamic Polymorphism** | Overriding (runtime)                | Frameworks, APIs           |
| **Abstract Class**       | Partial implementation              | Template pattern           |
| **Interface**            | Contract definition                 | Multiple capabilities      |

***

### Example: Complete Hierarchy

```java
interface Drawable {
    void draw();
}

abstract class Shape implements Drawable {
    protected String color;
    
    Shape(String color) { this.color = color; }
    
    // Concrete method
    public String getColor() { return color; }
    
    // Abstract method from interface
    // public abstract void draw(); // Inherited
    
    // Additional abstract method
    public abstract double area();
}

class Circle extends Shape {
    private double radius;
    
    Circle(double radius, String color) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " circle");
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public String toString() {
        return "Circle{radius=" + radius + ", color=" + color + "}";
    }
}
```

### Visual Inheritance Hierarchy

Here's how the complete inheritance hierarchy works with interfaces, abstract classes, and concrete implementations:

```mermaid
classDiagram
    class Drawable {
        <<interface>>
        +draw() void
    }
    
    class Shape {
        <<abstract>>
        #String color
        +Shape(String color)
        +getColor() String
        +area()* double
        +draw()* void
    }
    
    class Circle {
        -double radius
        +Circle(double radius, String color)
        +draw() void
        +area() double
        +toString() String
    }
    
    class Rectangle {
        -double width
        -double height
        +Rectangle(double w, double h, String color)
        +draw() void
        +area() double
        +toString() String
    }
    
    class Triangle {
        -double base
        -double height
        +Triangle(double b, double h, String color)
        +draw() void
        +area() double
        +toString() String
    }
    
    %% Inheritance relationships
    Drawable <|.. Shape : implements
    Shape <|-- Circle : extends
    Shape <|-- Rectangle : extends
    Shape <|-- Triangle : extends
    
    %% Method overriding indicators
    Circle : ⭐ Overrides draw()
    Circle : ⭐ Implements area()
    Rectangle : ⭐ Overrides draw()
    Rectangle : ⭐ Implements area()
    Triangle : ⭐ Overrides draw()
    Triangle : ⭐ Implements area()
```

**Java Inheritance Types Diagram:**
```mermaid
graph TD
    subgraph "Single Inheritance Model"
        Object["java.lang.Object\n(Root of all classes)"]
        
        subgraph "Interface Layer"
            I1["interface Serializable"]
            I2["interface Comparable"]
            I3["interface Drawable"]
        end
        
        subgraph "Abstract Layer"
            A1["abstract class Number"]
            A2["abstract class Shape"]
        end
        
        subgraph "Concrete Layer"
            C1["class Integer"]
            C2["class Double"]
            C3["class Circle"]
            C4["class Rectangle"]
        end
    end
    
    Object --> A1
    Object --> A2
    
    A1 --> C1
    A1 --> C2
    A2 --> C3
    A2 --> C4
    
    I1 -.->|implements| C1
    I2 -.->|implements| C1
    I3 -.->|implements| A2
    
    style Object fill:#e8f4fd
    style A1 fill:#fff2cc
    style A2 fill:#fff2cc
    style C1 fill:#d4edda
    style C2 fill:#d4edda
    style C3 fill:#d4edda
    style C4 fill:#d4edda
    style I1 fill:#f8d7da
    style I2 fill:#f8d7da
    style I3 fill:#f8d7da
```

**Method Resolution at Runtime:**
```mermaid
flowchart TD
    START["Animal animal = new Dog();"] --> CALL["animal.makeSound()"]
    CALL --> CHECK["JVM checks actual object type"]
    CHECK --> RUNTIME{"Runtime Type\nDetermination"}
    
    RUNTIME -->|Dog instance| DOG_METHOD["Calls Dog.makeSound()\nOutput: 'Woof!'"]
    RUNTIME -->|Cat instance| CAT_METHOD["Calls Cat.makeSound()\nOutput: 'Meow!'"]
    RUNTIME -->|Bird instance| BIRD_METHOD["Calls Bird.makeSound()\nOutput: 'Tweet!'"]
    
    subgraph "Compile Time"
        COMPILE["Compiler sees Animal reference\nVerifies makeSound() exists in Animal"]
    end
    
    subgraph "Runtime"
        RESOLVE["JVM resolves to actual implementation\nBased on object's real type"]
    end
    
    style START fill:#e3f2fd
    style DOG_METHOD fill:#c8e6c9
    style CAT_METHOD fill:#c8e6c9
    style BIRD_METHOD fill:#c8e6c9
    style COMPILE fill:#fff3e0
    style RESOLVE fill:#f3e5f5
```

***

### Callouts

> [!TIP]
> **Always override `equals()` and `hashCode()` together** using the same fields to maintain the contract.

> [!WARNING]
> **Final classes cannot be mocked in unit tests**—prefer composition over inheritance for testing flexibility.

> [!NOTE]
> **Use interfaces for "can-do" (capabilities)** and **abstract classes for "is-a" (relationships)**.

***

### Tags

#java #inheritance #polymorphism #final #abstract-classes #interfaces #object-methods #equals #hashcode #bestpractices

***

### See Also

- \[[Java Collections Framework]\]
- \[[Object-Oriented Design]\]
- \[[Functional Interfaces]\]
- \[[Unit Testing Java]\]


