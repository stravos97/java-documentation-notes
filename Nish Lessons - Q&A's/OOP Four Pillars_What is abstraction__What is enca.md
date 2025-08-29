---
tags: [java, oop, constructors, access-modifiers, date-time, cheatsheet]
date: 2025-08-29
topic: Java OOP, Constructors, and Access Control
---

# OOP Four Pillars

1. What is abstraction?
2. What is encapsulation?
3. What is inheritance?
4. What is polymorphism?
5. Date/Time
6. How do you work with date/time in Java?
7. Constructors
8. What does a constructor return?
9. What does the new keyword do when creating objects?
10. What is constructor overloading?
11. What is constructor chaining?
12. What is the default constructor?
13. Access Modifiers
14. What are private fields used for?
15. What is a protected field?
16. What are getters and setters used for?

## OOP Four Pillars

### Abstraction

- **Definition**: Hiding implementation details while showing only essential functionality to the user.[^1][^2]
- **Focus**: What an object does, not how it does it.
- **Implementation**: Through abstract classes (partial abstraction) or interfaces (complete abstraction).[^3]

```java
// Abstract class example
abstract class Shape {
    abstract void draw();           // Must be implemented
    void display() {               // Can have concrete methods
        System.out.println("Displaying shape");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a circle");
    }
}
```

> [!TIP]
> Use abstraction to create a template that forces subclasses to implement specific behavior while providing common functionality.

***

### Encapsulation

- **Definition**: Wrapping data and methods into a single unit (class) and controlling access to them.[^4][^5]
- **Implementation**: Using private fields with public getter/setter methods.
- **Benefits**: Data security, better code management, controlled access.[^4]

```java
class Employee {
    private String name;        // Private data
    private double salary;
    
    // Public getter
    public String getName() {
        return name;
    }
    
    // Public setter with validation
    public void setSalary(double salary) {
        if (salary > 0) {
            this.salary = salary;
        }
    }
}
```


***

### Inheritance

- **Definition**: Creating new classes based on existing ones, allowing code reuse.[^6][^7]
- **Syntax**: `class ChildClass extends ParentClass`
- **Benefits**: Code reusability, method overriding for polymorphism.[^7]

```java
// Parent class
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class
class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```


***

### Polymorphism

- **Definition**: "Many forms" - objects behaving differently based on their actual class type.[^8][^9]
- **Types**: Method overriding (runtime) and method overloading (compile-time).[^9]
- **Key Feature**: Same method call produces different behaviors.[^8]

```java
Animal myAnimal = new Dog();    // Reference type: Animal, Object type: Dog
myAnimal.sound();               // Calls Dog's sound() method - "Dog barks"
```


***

## Date/Time

### Working with Date/Time in Java

Modern Java uses the **`java.time`** package (Java 8+):[^10][^11]

| Class           | Purpose                | Example               |
|:----------------|:-----------------------|:----------------------|
| `LocalDate`     | Date only (yyyy-MM-dd) | `2025-08-29`          |
| `LocalTime`     | Time only (HH:mm:ss)   | `14:30:15`            |
| `LocalDateTime` | Date + Time            | `2025-08-29T14:30:15` |
| `ZonedDateTime` | Date + Time + Timezone | With timezone info    |

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

// Current date
LocalDate today = LocalDate.now();

// Current date and time
LocalDateTime now = LocalDateTime.now();

// Formatting
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
String formattedDate = today.format(formatter);
```

> [!NOTE]
> Avoid legacy `java.util.Date` and `java.util.Calendar` - use `java.time` package instead.

***

## Constructors

### What Does a Constructor Return?

- **Constructors don't have explicit return types**.[^12][^13]
- **Implicitly return**: A reference to the newly created object.
- **Java runtime**: Automatically handles the return process.

```java
class Demo {
    Demo() {  // No return type specified
        System.out.println("Constructor called");
    }
}

Demo obj = new Demo();  // Constructor returns reference to new Demo object
```


### What Does the `new` Keyword Do?

The `new` keyword:[^14][^15]

1. **Allocates memory** for the new object
2. **Calls the constructor** to initialize the object
3. **Returns a reference** to the created object
```java
Employee emp = new Employee("John");
//     ↑         ↑
// Reference   new keyword creates object and calls constructor
```


### Constructor Overloading

- **Definition**: Multiple constructors with different parameter lists.[^16][^17]
- **Purpose**: Provides flexibility in object creation.[^17]

```java
class Employee {
    private String name;
    private int id;
    
    // Default constructor
    public Employee() {
        this.name = "Unknown";
        this.id = 0;
    }
    
    // Constructor with name only
    public Employee(String name) {
        this.name = name;
        this.id = 0;
    }
    
    // Constructor with name and id
    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }
}
```


### Constructor Chaining

- **Purpose**: One constructor calling another constructor in the same class.
- **Syntax**: Use `this()` to call another constructor.

```java
class Employee {
    private String name;
    private int id;
    private double salary;
    
    public Employee() {
        this("Unknown", 0, 0.0);    // Calls 3-parameter constructor
    }
    
    public Employee(String name) {
        this(name, 0, 0.0);         // Calls 3-parameter constructor
    }
    
    public Employee(String name, int id, double salary) {
        this.name = name;
        this.id = id;
        this.salary = salary;
    }
}
```


### Default Constructor

- **Provided by Java** when no constructors are defined.
- **No parameters**, performs no initialization.
- **Disappears** once you define any constructor.

```java
// If no constructor is defined, Java provides:
class MyClass {
    public MyClass() {  // Default constructor
        // Empty body
    }
}
```


***

## Access Modifiers

### Private Fields

- **Purpose**: Hide data from outside access, enforce encapsulation.[^5][^4]
- **Access**: Only within the same class.
- **Best Practice**: Use private for data fields, provide controlled access via methods.

```java
class BankAccount {
    private double balance;     // Cannot be accessed directly from outside
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;  // Controlled access
        }
    }
}
```


### Protected Fields

- **Access Level**: Same package + subclasses (even in different packages).
- **Use Case**: When you want subclasses to access fields but not unrelated classes.

```java
class Vehicle {
    protected String engine;    // Accessible by subclasses
}

class Car extends Vehicle {
    void showEngine() {
        System.out.println(engine);  // Can access protected field
    }
}
```


### Getters and Setters

- **Purpose**: Controlled access to private fields.[^5][^4]
- **Benefits**: Validation, computed properties, debugging.

```java
class Student {
    private String name;
    private int age;
    
    // Getter
    public String getName() {
        return name;
    }
    
    // Setter with validation
    public void setAge(int age) {
        if (age >= 0 && age <= 150) {
            this.age = age;
        } else {
            throw new IllegalArgumentException("Invalid age");
        }
    }
}
```


***

## Quick Reference Table

| Concept                     | Key Points                              | Usage                            |
|:----------------------------|:----------------------------------------|:---------------------------------|
| **Abstraction**             | Hide implementation, show functionality | Abstract classes, interfaces     |
| **Encapsulation**           | Private fields + public methods         | Data security, controlled access |
| **Inheritance**             | `extends` keyword, code reuse           | IS-A relationships               |
| **Polymorphism**            | Many forms, method overriding           | Runtime behavior differences     |
| **Date/Time**               | `java.time` package (modern)            | `LocalDate`, `LocalDateTime`     |
| **Constructors**            | No return type, initialize objects      | Object creation and setup        |
| **`new` keyword**           | Allocates memory, calls constructor     | Object instantiation             |
| **Constructor Overloading** | Multiple constructors, different params | Flexible object creation         |
| **Constructor Chaining**    | `this()` calls other constructors       | Code reuse within class          |
| **Private**                 | Class-only access                       | Data hiding                      |
| **Protected**               | Package + subclass access               | Inheritance scenarios            |
| **Getters/Setters**         | Controlled field access                 | Encapsulation implementation     |

***

## Access Modifier Summary

| Modifier    | Same Class | Same Package | Subclass | Everywhere |
|:------------|:-----------|:-------------|:---------|:-----------|
| `private`   | ✅          | ❌            | ❌        | ❌          |
| `default`   | ✅          | ✅            | ❌        | ❌          |
| `protected` | ✅          | ✅            | ✅        | ❌          |
| `public`    | ✅          | ✅            | ✅        | ✅          |

***

> [!WARNING]
> Always prefer private fields with public getters/setters for proper encapsulation.

> [!EXAMPLE]
> Complete OOP example combining all four pillars:
> ```java
> // Abstraction through abstract class
> abstract class Shape {
>     protected String color;  // Protected for inheritance
> 
>     abstract double getArea(); // Abstract method
> }
> 
> // Inheritance and Encapsulation
> class Circle extends Shape {
>     private double radius;   // Encapsulation
> 
>     public Circle(double radius) {  // Constructor
>         this.radius = radius;
>     }
> 
>     @Override
>     double getArea() {      // Polymorphism
>         return Math.PI * radius * radius;
>     }
> 
>     // Getter/Setter for encapsulation
>     public double getRadius() { return radius; }
>     public void setRadius(double radius) { this.radius = radius; }
> }
> ```

***

\#java \#oop \#abstraction \#encapsulation \#inheritance \#polymorphism \#constructors \#access-modifiers \#date-time \#bestpractices

⁂

[^1]: https://www.geeksforgeeks.org/java/abstraction-in-java-2/

[^2]: https://www.geekster.in/articles/abstraction-in-java/

[^3]: https://www.crio.do/blog/abstraction-in-java/

[^4]: https://www.geeksforgeeks.org/java/encapsulation-in-java/

[^5]: https://www.simplilearn.com/tutorials/java-tutorial/java-encapsulation

[^6]: https://www.geeksforgeeks.org/java/inheritance-in-java/

[^7]: https://dspmuranchi.ac.in/pdf/Blog/Inheritance and its Types.pdf

[^8]: https://www.w3schools.com/java/java_polymorphism.asp

[^9]: https://www.geeksforgeeks.org/java/polymorphism-in-java/

[^10]: https://www.w3schools.com/java/java_date.asp

[^11]: https://howtodoinjava.com/java/date-time/intro-to-date-time-api/

[^12]: https://www.digitalocean.com/community/tutorials/constructor-in-java

[^13]: https://stackoverflow.com/questions/14737420/what-does-a-constructor-return-in-java

[^14]: https://www.geeksforgeeks.org/java/different-ways-create-objects-java/

[^15]: https://www.geekster.in/articles/java-new-keyword/

[^16]: https://www.geeksforgeeks.org/java/constructor-overloading-java/

[^17]: https://dev.to/emleons/constructor-overloading-in-java-1c53

[^18]: https://www.w3schools.com/java/java_abstract.asp

[^19]: https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html

