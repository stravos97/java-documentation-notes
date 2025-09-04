---
tags: [java, oop, core-concepts, study-guide, cheatsheet]
date: 2023-10-15
topic: Java Core Concepts Study Guide
---

# Java Core Concepts Study Guide

1. What are the fundamental components of a Java class?
2. How does inheritance work in Java and what are its limitations?
3. What is the difference between interfaces and abstract classes?
4. How do method overloading and overriding differ?
5. What is the contract between `equals()` and `hashCode()` methods?
6. How does Java handle primitive types versus objects in memory?
7. What is the difference between stack and heap memory?
8. How does the String pool work and why is it important?
9. What are the key differences between checked and unchecked exceptions?
10. How does the Garbage Collector work in Java?
11. How do you properly declare constants in Java?
12. What are the best practices for JUnit testing?

***

## Core Java Concepts

### OOP Fundamentals

#### Class Structure

A Java class serves as a blueprint for creating objects. Every standalone Java program must have at least one class.

**Key Components:**

| Component | Description | Access Control |
|:----------|:------------|:---------------|
| Fields (Attributes) | Represent the state/data of objects | Typically `private` for encapsulation |
| Methods | Define behaviors/actions objects can perform | `public`, `protected`, `private`, or default |
| Constructors | Special methods for initializing new objects | Usually `public` |
| Access Modifiers | Control visibility and accessibility | `public`, `protected`, `private`, `default` |

**Important Keywords:**
- `final`: Prevents variables, methods, or classes from being changed or overridden
- `static`: Members belong to the class itself, not to individual objects
- `this`: Refers to the current object within a constructor or method

> [!TIP]
> Constructor overloading allows for flexible object creation by providing multiple constructors with different parameter lists. Use the `this` keyword to distinguish between class attributes and constructor parameters when they share the same name.

#### Inheritance & Polymorphism

Java supports single inheritance through the `extends` keyword, meaning a class can extend only one parent class.

**Key Concepts:**
- **Inheritance**: Creating a subclass that inherits from a superclass (models "is-a" relationship)
- **Method Overriding**: Providing a subclass-specific implementation for a method defined in the superclass (runtime polymorphism)
- **Abstract Classes**: Non-instantiable base types that provide partial implementation

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark!");
    }
}
```

> [!WARNING]
> Java does not support multiple class inheritance to avoid the "diamond problem." However, a class can implement multiple interfaces to achieve similar functionality.

**Abstract Methods & Classes:**
- Abstract methods have a signature but no implementation
- Any class with at least one abstract method must be declared `abstract`
- Abstract methods cannot be `private`, `static`, or `final`
- First concrete subclass must implement all abstract methods

#### Interfaces

Interfaces define a "can do" contract that implementing classes must fulfill.

**Key Features:**
- Classes implement interfaces using the `implements` keyword
- A class can implement multiple interfaces (solving the multiple inheritance problem)
- All methods in an interface are implicitly `public`
- Default methods (Java 8+) provide implementation that can be inherited
- Static methods (Java 8+) are utility methods called via the interface name
- Private methods (Java 9+) encapsulate reusable code within interfaces

> [!NOTE]
> When a class implements multiple interfaces with conflicting default methods, the class must explicitly override the method and specify which implementation to use with syntax like `InterfaceName.super.method()`.

#### Method Overloading vs. Overriding

| Feature | Overloading | Overriding |
|:--------|:------------|:-----------|
| Definition | Multiple methods with same name but different parameters in the same class | Subclass provides implementation for method already defined in superclass |
| Also Known As | Compile-time polymorphism | Runtime polymorphism |
| Parameter List | Must differ in type, number, or order | Must be identical |
| Return Type | Can be different | Must be same or covariant |
| Access Modifier | Can be different | Cannot be more restrictive |
| `@Override` | Not used | Recommended annotation |
| Resolution Time | Compile time | Runtime |

> [!EXAMPLE]
> ```java
> // Overloading example
> public class Calculator {
>     public int add(int a, int b) {
>         return a + b;
>     }
>     
>     public double add(double a, double b) {
>         return a + b;
>     }
> }
> 
> // Overriding example
> public class Vehicle {
>     public void startEngine() {
>         System.out.println("Vehicle engine starting");
>     }
> }
> 
> public class Car extends Vehicle {
>     @Override
>     public void startEngine() {
>         System.out.println("Car engine starting with key");
>     }
> }
> ```

#### `equals()` and `hashCode()`

These methods, inherited from the `Object` class, are crucial for proper object comparison and hash-based collections.

**The Contract:**
- If two objects are equal according to `equals()`, their `hashCode()` must return the same integer
- `equals()` must be:
  - **Reflexive**: `x.equals(x)` must return true
  - **Symmetric**: `x.equals(y)` implies `y.equals(x)`
  - **Transitive**: If `x.equals(y)` and `y.equals(z)`, then `x.equals(z)`
  - **Consistent**: Multiple invocations return same result if objects don't change
  - **Non-null**: `x.equals(null)` must return false

> [!WARNING]
> Always override both `equals()` and `hashCode()` together, using the same fields for comparison. Failing to maintain this contract will cause problems with hash-based collections like `HashMap` and `HashSet`.

**Important Distinction:**
- `==` operator compares object references (memory addresses)
- `.equals()` method compares object content (logical equality)

***

### Data Types & Memory

#### Primitive Types & Pass-by-Value

Java has eight primitive data types:
- `byte`, `short`, `int`, `long` (integer types)
- `float`, `double` (floating-point types)
- `char` (character type)
- `boolean` (true/false values)

**Pass-by-Value Principle:**
- Java is always pass-by-value
- For primitives, the actual value is copied
- Changes to method parameters don't affect original variables

> [!EXAMPLE]
> ```java
> public class PassByValueExample {
>     public static void main(String[] args) {
>         int x = 10;
>         modify(x);
>         System.out.println(x); // Still prints 10
>     }
>     
>     public static void modify(int value) {
>         value = value + 5;
>     }
> }
> ```

#### Heap vs. Stack Memory

| Memory Area | Stores | Characteristics | Lifetime |
|:------------|:-------|:----------------|:---------|
| **Stack** | Primitive values, local variables, method call frames, object references | Fast access, LIFO structure, limited size (typically 1MB) | Cleared when method exits |
| **Heap** | Objects, arrays, instance variables, static variables | Slower access, larger size, managed by GC | Until objects are no longer referenced |

> [!NOTE]
> Each thread has its own stack, but the heap is shared across all threads. Static variables reside in the Method Area, which is considered part of the heap in modern JVMs.

#### String Pool

The String Constant Pool (SCP) is a special area in heap memory that stores string literals.

**String Creation:**
- String literals (`"text"`) are stored in the SCP
- `new String("text")` creates a new object in regular heap memory, even if identical string exists in SCP

**Immutability:**
- String objects are immutable
- Operations like `concat()`, `substring()`, `replace()` return new String objects

**Comparison:**
- `==` compares memory addresses (references)
- `.equals()` compares actual content (character sequence)

> [!EXAMPLE]
> ```java
> String s1 = "hello";
> String s2 = "hello";
> String s3 = new String("hello");
> 
> System.out.println(s1 == s2); // true (same object in SCP)
> System.out.println(s1 == s3); // false (different objects)
> System.out.println(s1.equals(s3)); // true (same content)
> ```

> [!TIP]
> Prefer string literals over `new String()` for memory efficiency, as the JVM reuses strings from the SCP.

***

### Control Flow & Operators

#### Ternary Operator

The ternary operator provides a concise alternative to simple if-else statements.

**Syntax:** `condition ? expressionIfTrue : expressionIfFalse`

> [!EXAMPLE]
> ```java
> int x = 10;
> String result = (x > 5) ? "Greater than 5" : "Less than or equal to 5";
> ```

> [!WARNING]
> Avoid nesting ternary operators or using them for complex logic, as this can reduce code readability.

#### Increment Operators

Java provides prefix and postfix increment/decrement operators.

| Operator | Behavior | Example |
|:---------|:---------|:--------|
| Prefix (`++i`) | Increment first, then use value | `int y = ++x;` (x incremented before assignment) |
| Postfix (`i++`) | Use current value first, then increment | `int y = x++;` (x incremented after assignment) |

> [!NOTE]
> In loops, prefix increment is slightly more efficient, but the difference is usually negligible. Consistency in style is often more important.

#### Switch Statements

The `switch` statement provides multi-branch selection based on a single expression.

**Supported Types:** `byte`, `short`, `char`, `int`, corresponding wrapper classes, `String` (Java 7+), and `enum` types.

> [!EXAMPLE]
> ```java
> String day = "MONDAY";
> 
> switch(day) {
>     case "MONDAY":
>     case "TUESDAY":
>         System.out.println("Weekday");
>         break;
>     case "SATURDAY":
>     case "SUNDAY":
>         System.out.println("Weekend");
>         break;
>     default:
>         System.out.println("Invalid day");
> }
> ```

> [!WARNING]
> Without `break` statements, execution will "fall through" to subsequent cases. This is often a source of bugs unless intentionally used.

***

### Arrays & Collections

#### Arrays

Arrays are fixed-size container objects that hold values of a single type.

**Key Characteristics:**
- Zero-indexed (first element at index 0, last at length-1)
- Length is fixed after creation
- Contents are mutable
- Stored on the heap as reference types

> [!EXAMPLE]
> ```java
> // Declaration and initialization
> int[] numbers = new int[5];
> String[] names = {"Alice", "Bob", "Charlie"};
> 
> // Accessing elements
> int first = numbers[0];
> int last = numbers[numbers.length - 1];
> ```

> [!TIP]
> When iterating through an array to compare adjacent elements, use `i < arr.length - 1` as the loop condition to avoid `ArrayIndexOutOfBoundsException`.

#### String.split()

The `split(String regex)` method divides a string into an array of substrings based on a regular expression.

> [!EXAMPLE]
> ```java
> String sentence = "Hello world, how are you?";
> String[] words = sentence.split("\\s+"); // Split by one or more whitespace characters
> System.out.println(words.length); // Outputs: 5
> ```

> [!NOTE]
> The regex `\\s+` matches one or more whitespace characters (spaces, tabs, newlines). Remember that backslashes must be escaped in Java strings.

***

### Exceptions

#### Checked vs. Unchecked Exceptions

| Type | Extends | Handling Requirement | Examples |
|:-----|:--------|:---------------------|:---------|
| **Checked Exceptions** | `Exception` (not `RuntimeException`) | Must be handled or declared | `IOException`, `SQLException`, `FileNotFoundException` |
| **Unchecked Exceptions** | `RuntimeException` | Optional handling | `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException` |

> [!WARNING]
> Checked exceptions will cause compilation errors if not handled or declared with `throws`. Unchecked exceptions don't require explicit handling but often indicate programming errors that should be fixed.

#### General Exception Handling

Java provides structured mechanisms for exception handling:

```java
try {
    // Code that might throw an exception
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // Handle specific exception
    System.out.println("Cannot divide by zero");
} catch (Exception e) {
    // Handle general exceptions
    e.printStackTrace();
} finally {
    // Always executed, regardless of exception
    System.out.println("Cleanup code here");
}
```

**Best Practices:**
- Catch specific exceptions before general ones
- Use `finally` for resource cleanup (closing files, database connections)
- Use `throws` to declare checked exceptions a method might throw
- Use `throw` to explicitly throw exceptions

> [!TIP]
> In Java 7+, use try-with-resources for automatic resource management when working with AutoCloseable resources:
> ```java
> try (FileReader reader = new FileReader("file.txt")) {
>     // Use the resource
> } catch (IOException e) {
>     // Handle exception
> }
> ```

***

### Advanced Topics

#### Garbage Collector (GC)

The Garbage Collector automatically manages heap memory by:

1. **Mark Phase**: Identifying all reachable ("live") objects starting from GC roots
2. **Sweep Phase**: Reclaiming memory occupied by unreachable ("dead") objects
3. **Compact Phase**: Moving live objects to reduce heap fragmentation

**GC Eligibility:**
- When a reference is set to `null`
- When a reference is reassigned to another object
- When local references go out of scope (method ends)

> [!NOTE]
> The JVM decides when to run GC; it's automatic and periodic. While `System.gc()` is a hint, there's no guarantee the GC will run immediately.

#### Declaring Constants

Constants in Java are declared using `static final` keywords:

```java
public static final int MAX_SIZE = 100;
public static final double PI = 3.14159;
```

**Naming Convention:** ALL_CAPS_WITH_UNDERSCORES

> [!TIP]
> While `static final` constants are useful, enums are often a more type-safe and powerful alternative for predefined sets of constants, especially when they need associated behavior or data.

#### Parameterized Tests

JUnit 5 provides parameterized tests to run the same test logic with different inputs:

```java
@ParameterizedTest
@ValueSource(strings = {"apple", "banana", "cherry"})
void testStringLength(String word) {
    assertTrue(word.length() > 0);
}

@ParameterizedTest
@CsvSource({
    "John, 30",
    "Jane, 25",
    "Bob, 40"
})
void testPerson(String name, int age) {
    assertEquals(age, getAge(name));
}
```

**Data Sources:**
- `@ValueSource`: For single-parameter tests
- `@CsvSource`: For multiple parameters using comma-separated values
- `@CsvFileSource`: For reading test data from CSV files

***

## Testing (JUnit)

### Structure of a JUnit Test Case

JUnit 5 provides annotations for test lifecycle management:

```java
class CalculatorTest {
    
    private Calculator calculator;
    
    @BeforeAll
    static void setupClass() {
        // Runs once before all tests
    }
    
    @BeforeEach
    void setup() {
        // Runs before each test
        calculator = new Calculator();
    }
    
    @Test
    void testAddition() {
        assertEquals(5, calculator.add(2, 3));
    }
    
    @Test
    void testDivisionByZero() {
        assertThrows(ArithmeticException.class, () -> calculator.divide(10, 0));
    }
    
    @AfterEach
    void tearDown() {
        // Runs after each test
        calculator = null;
    }
    
    @AfterAll
    static void tearDownClass() {
        // Runs once after all tests
    }
}
```

**Execution Order:**
1. `@BeforeAll` (once)
2. Constructor (per test)
3. `@BeforeEach` (per test)
4. `@Test` method
5. `@AfterEach` (per test)
6. `@AfterAll` (once)

> [!TIP]
> Best practices for JUnit testing:
> - Test boundary values (empty arrays, single elements, min/max values)
> - Avoid loops in tests (prefer parameterized tests)
> - Ensure test independence (don't rely on execution order)
> - Use descriptive test names with `@DisplayName`
