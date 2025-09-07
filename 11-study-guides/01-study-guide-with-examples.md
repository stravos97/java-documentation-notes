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

## `equals()` and `hashCode()`

### Understanding the Contract

The `equals()` and `hashCode()` methods, inherited from the `Object` class, form a critical contract that must be properly implemented for your objects to work correctly with Java's collection framework, especially hash-based collections.

**The Core Contract:**
> [!WARNING]
> **If two objects are equal according to `equals()`, they MUST return the same hash code from `hashCode()`.**  
> The reverse is not required (different objects can have the same hash code), but having distinct hash codes for distinct objects improves performance in hash-based collections.

### Key Principles

#### `equals()` Method
- Determines **logical equality** between objects (content-based comparison)
- Default implementation uses reference equality (same as `==` operator)
- Must be overridden to provide meaningful comparison of object contents
- Follows specific mathematical properties:

| Property | Definition | Example |
|:---------|:-----------|:--------|
| **Reflexive** | `x.equals(x)` must return `true` | An object must equal itself |
| **Symmetric** | `x.equals(y)` implies `y.equals(x)` | If x equals y, then y must equal x |
| **Transitive** | If `x.equals(y)` and `y.equals(z)`, then `x.equals(z)` | Equality must be consistent across comparisons |
| **Consistent** | Multiple calls return same result if objects don't change | No random behavior in equality checks |
| **Non-null** | `x.equals(null)` must return `false` | Objects are never equal to null |

#### `hashCode()` Method
- Returns an integer hash code used primarily by hash-based collections
- Must produce the **same integer** for objects that are equal
- Should produce **distinct integers** for unequal objects (as much as possible)
- Must be consistent during a single execution (though can vary between executions)

### Implementation Guidelines

> [!TIP]
> **Always override both methods together** - never implement just one without the other. They must use the **same fields** for comparison to maintain the contract.

#### Example Implementation
```java
public class Person {
    private String name;
    private int age;
    private String email;
    
    @Override
    public boolean equals(Object o) {
        // 1. Check if same reference
        if (this == o) return true;
        
        // 2. Check for null and type mismatch
        if (o == null || getClass() != o.getClass()) return false;
        
        // 3. Cast and compare relevant fields
        Person person = (Person) o;
        return age == person.age &&
               Objects.equals(name, person.name) &&
               Objects.equals(email, person.email);
    }
    
    @Override
    public int hashCode() {
        // Use same fields as in equals()
        return Objects.hash(name, age, email);
    }
}
```

#### Common Implementation Patterns

**Using `Objects` Utility Class (Recommended):**
```java
@Override
public int hashCode() {
    return Objects.hash(field1, field2, field3);
}
```

**Manual Implementation (for understanding):**
```java
@Override
public int hashCode() {
    int result = 17;
    result = 31 * result + (name != null ? name.hashCode() : 0);
    result = 31 * result + age;
    result = 31 * result + (email != null ? email.hashCode() : 0);
    return result;
}
```

### Critical Differences: `==` vs `.equals()`

| Feature | `==` Operator | `.equals()` Method |
|:--------|:--------------|:-------------------|
| **Type** | Operator | Method |
| **Comparison** | Reference equality (memory address) | Logical equality (object content) |
| **Overridable** | No | Yes |
| **Default Behavior** | Compares memory addresses | Same as `==` (reference equality) |
| **Use Case** | Checking if two references point to same object | Checking if two objects have same content |

> [!EXAMPLE]
> ```java
> String s1 = new String("hello");
> String s2 = new String("hello");
> 
> System.out.println(s1 == s2);      // false (different objects)
> System.out.println(s1.equals(s2)); // true (same content)
> ```

### Why This Matters in Collections

**Hash-based collections (HashMap, HashSet, etc.) rely on this contract:**

1. When you add an object to a `HashSet` or as a key in `HashMap`:
   - The collection first computes the object's `hashCode()`
   - Uses this hash to determine which "bucket" to store the object in
   - When checking for duplicates, it only compares objects in the same bucket using `equals()`

2. **If you violate the contract:**
   - Objects might be stored in the wrong bucket
   - `contains()`, `get()`, and other methods might fail to find objects
   - Data can effectively "disappear" from collections

> [!WARNING]
> **Real-world consequence:** If you use a custom object as a key in a `HashMap` without properly overriding `equals()` and `hashCode()`, you might not be able to retrieve values using what appears to be the same key.

### Common Implementation Mistakes

> [!WARNING]
> **These are frequent exam questions and real-world bugs:**

1. **Overriding only one method** - Implementing `equals()` without `hashCode()` (or vice versa)
2. **Using different fields** - Using different fields in `equals()` and `hashCode()`
3. **Inconsistent null handling** - Not properly handling null values in comparisons
4. **Breaking symmetry** - Comparing with superclass objects incorrectly
5. **Using mutable fields** - Using fields that can change after the object is added to a collection

### Best Practices

> [!TIP]
> **For exam success and real-world coding:**

- Always override both methods together
- Use the same fields in both implementations
- Prefer using `Objects.equals()` and `Objects.hash()` for null-safe comparisons
- For immutable objects, consider caching the hash code for performance
- Document your equality semantics clearly
- Test your implementations thoroughly with edge cases
- If using an IDE, leverage code generation features but review the generated code

### Memory Diagram: How Hash Collections Work

```
HashMap Buckets (Array)
┌─────────┐
│ Bucket 0│ → [KeyA, ValueA] → [KeyD, ValueD]
├─────────┤
│ Bucket 1│ → [KeyB, ValueB]
├─────────┤
│ Bucket 2│ → [KeyC, ValueC]
└─────────┘

1. Calculate hashCode() for KeyX
2. Determine bucket index = hashCode % number_of_buckets
3. Search only within that bucket using equals()
```

> [!NOTE]
> This is why proper `hashCode()` implementation is crucial - a poor implementation that sends all objects to the same bucket effectively turns a `HashMap` into a linked list, degrading performance from O(1) to O(n).
***

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

#### String.split() Without Regex

While `String.split()` takes a regex pattern as its delimiter, there are cases where you want to use a literal string without regex interpretation.

**Special Characters in Delimiters:**
- Characters like `.`, `|`, `*`, `+` have special meaning in regex
- To use them as literal delimiters, they must be escaped with backslashes

> [!EXAMPLE]
> ```java
> // Splitting by a period (which is a regex wildcard character)
> String url = "www.example.com";
> String[] parts = url.split("\\."); // Double backslash to escape the period
> // Result: ["www", "example", "com"]
> 
> // Splitting by a pipe character (which is regex OR operator)
> String data = "apple|banana|cherry";
> String[] items = data.split("\\|");
> // Result: ["apple", "banana", "cherry"]
> ```

***

### Exceptions

#### Checked vs. Unchecked Exceptions

| Type | Extends | Handling Requirement | Examples |
|:-----|:--------|:---------------------|:---------|
| **Checked Exceptions** | `Exception` (not `RuntimeException`) | Must be handled or declared | `IOException`, `SQLException`, `FileNotFoundException` |
| **Unchecked Exceptions** | `RuntimeException` | Optional handling | `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException` |

> [!WARNING]
> Checked exceptions will cause compilation errors if not handled or declared with `throws`. Unchecked exceptions don't require explicit handling but often indicate programming errors that should be fixed.

#### Using `throw` and `throws` Keywords

**`throws` Keyword:**
- Used in method signature to declare checked exceptions a method might throw
- Tells the caller that they need to handle these exceptions

```java
public void readFile(String filePath) throws IOException {
    // Code that might throw IOException
    FileReader reader = new FileReader(filePath);
    BufferedReader bufferedReader = new BufferedReader(reader);
    // Process file...
}
```

> [!EXAMPLE]
> ```java
> public class FileProcessor {
>     // This method declares it might throw IOException
>     public void processFile(String filename) throws IOException {
>         // File operations that might throw IOException
>         try (FileReader reader = new FileReader(filename)) {
>             // Process the file
>         }
>     }
> }
> ```

**`throw` Keyword:**
- Used to explicitly throw an exception from within a method
- Creates and throws an instance of an exception class

```java
public void validateAge(int age) {
    if (age < 0) {
        // Explicitly throwing an exception
        throw new IllegalArgumentException("Age cannot be negative");
    }
    // Continue validation...
}
```

> [!EXAMPLE]
> ```java
> public class UserValidator {
>     // Custom exception class
>     public static class InvalidUsernameException extends Exception {
>         public InvalidUsernameException(String message) {
>             super(message);
>         }
>     }
>     
>     // Method that both declares and throws exceptions
>     public void validateUsername(String username) throws InvalidUsernameException {
>         if (username == null || username.isEmpty()) {
>             throw new InvalidUsernameException("Username cannot be null or empty");
>         }
>         
>         if (username.length() < 3) {
>             throw new InvalidUsernameException("Username must be at least 3 characters");
>         }
>     }
> }
> ```

> [!TIP]
> **Best Practices for Exception Handling:**
> - Use `throws` to declare checked exceptions that the method doesn't handle itself
> - Use `throw` to explicitly signal exceptional conditions in your code
> - For custom exceptions, extend appropriate base classes:
>   - `Exception` for checked exceptions
>   - `RuntimeException` for unchecked exceptions
> - Always include meaningful error messages when throwing exceptions

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

JUnit 5 provides a powerful framework for writing unit tests in Java with a well-defined test lifecycle.

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

**Execution Order (MEMORIZE):**
1. `@BeforeAll` (static, once)
2. Constructor (per test)
3. `@BeforeEach` (per test)
4. `@Test` method
5. `@AfterEach` (per test)
6. `@AfterAll` (static, once)

> [!TIP]
> **Best practices for JUnit testing:**
> - Test boundary values (empty arrays, single elements, min/max values)
> - Avoid loops in tests (prefer parameterized tests)
> - Ensure test independence (don't rely on execution order)
> - Use descriptive test names with `@DisplayName`
> - Apply the Arrange-Act-Assert (AAA) pattern:
>   ```java
>   @Test
>   void calculateDiscount_ValidCustomer_ReturnsDiscountedPrice() {
>       // Arrange - set up test data
>       Customer customer = new Customer(true);
>       Product product = new Product("Book", 20.00);
>       
>       // Act - execute the behavior
>       double discountedPrice = calculator.applyDiscount(customer, product);
>       
>       // Assert - verify results
>       assertEquals(18.00, discountedPrice);
>   }
>   ```

### Hamcrest Matchers Integration

Hamcrest provides more expressive and readable assertions compared to standard JUnit assertions.

> [!WARNING]
> **Essential Import Statement:**
> ```java
> import static org.hamcrest.MatcherAssert.assertThat;
> import static org.hamcrest.Matchers.*;
> ```
> JUnit 5 doesn't include its own `assertThat` method - you must use Hamcrest's.

#### Basic Value Matching

```java
@Test
void basicMatchers() {
    String value = "Hello World";
    
    // Traditional JUnit
    assertEquals("Hello World", value);
    assertNotNull(value);
    
    // Hamcrest equivalents
    assertThat(value, is("Hello World"));
    assertThat(value, equalTo("Hello World"));
    assertThat(value, not(emptyString()));
    assertThat(value, notNullValue());
}
```

> [!EXAMPLE]
> ```java
> // Traditional JUnit vs Hamcrest
> assertTrue(result instanceof String);
> assertEquals(5, list.size());
> assertTrue(text.contains("expected"));
> 
> // Hamcrest - more expressive
> assertThat(result, instanceOf(String.class));
> assertThat(list, hasSize(5));
> assertThat(text, containsString("expected"));
> ```

#### String Matchers

```java
@Test
void stringMatchers() {
    String text = "The quick brown fox jumps over the lazy dog";
    
    assertThat(text, startsWith("The"));
    assertThat(text, endsWith("dog"));
    assertThat(text, containsString("fox"));
    assertThat(text, containsStringIgnoringCase("QUICK"));
    assertThat(text, stringContainsInOrder("quick", "fox", "lazy"));
}
```

#### Collection Matchers

```java
@Test
void collectionMatchers() {
    List<String> fruits = List.of("apple", "banana", "orange", "grape");
    
    assertThat(fruits, hasSize(4));
    assertThat(fruits, hasItem("banana"));
    assertThat(fruits, hasItems("apple", "orange"));
    assertThat(fruits, containsInAnyOrder("grape", "apple", "orange", "banana"));
    assertThat(fruits, not(hasItem("mango")));
    assertThat(fruits, everyItem(notNullValue()));
}
```

#### Combining Matchers

```java
@Test
void combinedMatchers() {
    String value = "JUnit Testing";
    
    // All conditions must be true
    assertThat(value, allOf(
        startsWith("JUnit"),
        containsString("Test"),
        not(endsWith("Framework"))
    ));
    
    // Any condition can be true  
    assertThat(value, anyOf(
        equalTo("Testing"),
        containsString("JUnit"),
        startsWith("Unit")
    ));
}
```

> [!TIP]
> **JUnit vs Hamcrest Comparison:**
> 
> | JUnit Assertion | Hamcrest Equivalent | Notes |
> |:----------------|:--------------------|:------|
> | `assertEquals(expected, actual)` | `assertThat(actual, equalTo(expected))` | Note parameter order difference |
> | `assertTrue(condition)` | `assertThat(condition, is(true))` | More readable |
> | `assertNotNull(object)` | `assertThat(object, notNullValue())` | Better error messages |
> | `assertArrayEquals(expected, actual)` | `assertThat(actual, arrayContaining(expected))` | More flexible |

### Creating Parameterized Tests

JUnit 5 provides powerful parameterized testing capabilities to run the same test logic with different input values.

```java
@ParameterizedTest
@ValueSource(ints = {2, 4, 6, 8})
@DisplayName("Should return true for even numbers")
void isEven_ReturnsTrue_ForEvenNumbers(int number) {
    assertTrue(MathUtils.isEven(number));
}
```

#### Data Sources

**`@ValueSource`:** Used for tests requiring a single parameter:

```java
@ParameterizedTest
@ValueSource(strings = {"hello", "world", "junit"})
void shouldContainOnlyLetters(String word) {
    assertTrue(word.matches("[a-zA-Z]+"));
}
```

**`@CsvSource`:** Used for tests requiring multiple parameters:

```java
@ParameterizedTest
@CsvSource({
    "1, 1, 2",
    "2, 3, 5", 
    "5, 7, 12"
})
@DisplayName("Should calculate correct sum")
void add_ReturnsCorrectSum(int a, int b, int expectedSum) {
    assertEquals(expectedSum, calculator.add(a, b));
}

// Using text blocks (Java 15+)
@ParameterizedTest
@CsvSource(textBlock = """
    Good morning!,    8
    Good afternoon!, 15  
    Good evening!,   21
    """)
void getGreeting_ReturnsAppropriateGreeting(String expected, int time) {
    assertEquals(expected, TimeService.getGreeting(time));
}
```

#### System Under Test (SUT) Pattern

Your code demonstrates the SUT pattern effectively:

```java
class CalculatorTests {
    private Calculator sut;  // System Under Test
    
    @BeforeEach
    void setUp() {
        sut = new Calculator(10, 5);
    }
    
    @Test
    void add_ReturnsCorrectSum() {
        // Given - setup in @BeforeEach
        
        // When  
        double result = sut.add();
        
        // Then
        assertEquals(15.0, result);
    }
}
```

> [!WARNING]
> **Test Execution Order with `@Order`:**
> 
> ```java
> @TestMethodOrder(OrderAnnotation.class)
> class OrderedIntegrationTests {
>     
>     @Test
>     @Order(1)
>     void createUser() {
>         // Must run first
>     }
>     
>     @Test  
>     @Order(2)
>     void updateUser() {
>         // Runs second
>     }
>     
>     @Test
>     @Order(3) 
>     void deleteUser() {
>         // Runs last
>     }
> }
> ```
> 
> **Important:** Tests should generally be independent and not rely on execution order. Use `@Order` sparingly, primarily for integration tests.

### Exception Testing

The modern JUnit 5 approach for exception testing using `assertThrows()`:

```java
@Test
@DisplayName("Should throw IllegalArgumentException for negative age")
void setAge_ThrowsException_WhenAgeIsNegative() {
    Animal animal = new Animal();
    
    // Returns the thrown exception for further assertions
    IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class, 
        () -> animal.setAge(-5)
    );
    
    assertEquals("Age must not be negative: -5", exception.getMessage());
}
```

**Key Points:**
- Use lambda expressions `() -> methodCall()` for cleaner syntax
- Always verify exception messages when meaningful
- Test one exception per test method for clarity
- `assertThrows()` returns the exception for additional assertions

> [!WARNING]
> **Common Exam Mistakes to Avoid:**
> - Using `@BeforeEach` with static methods (compilation error!)
> - Confusing expected vs actual in `assertEquals(actual, expected)` (should be `assertEquals(expected, actual)`)
> - Forgetting lambda in `assertThrows`: `assertThrows(Exception.class, methodCall())` (should be `assertThrows(Exception.class, () -> methodCall())`)
> - Using `@Test` instead of `@ParameterizedTest` with parameter sources
> - Using wrong Hamcrest import: `import static org.junit.jupiter.api.Assertions.assertThat` (should be Hamcrest's)
***

***
## Coding Questions (4 Key Tasks)

### Password Size Evaluator

**Task:** Implement logic to check if a password string meets length requirements.

```java
public String evaluatePassword(String password) {
    int length = password.length();
    
    if (length < 8) {
        return "Too short";
    } else if (length > 20) {
        return "Too long";
    } else {
        return "Valid";
    }
    
    // Using ternary operator (less readable for multiple conditions)
    // return (length < 8) ? "Too short" : (length > 20) ? "Too long" : "Valid";
}
```

> [!NOTE]
> The ternary operator can be used for simple conditions, but for multiple conditions like this, if-else statements are more readable.

### Implement an Interface

**Task:** Create a class that implements an interface.

```java
// Interface definition
public interface Validator {
    boolean isValid(String input);
    String getErrorMessage();
}

// Implementation
public class PasswordValidator implements Validator {
    private static final int MIN_LENGTH = 8;
    private static final int MAX_LENGTH = 20;
    
    @Override
    public boolean isValid(String password) {
        return password != null && 
               password.length() >= MIN_LENGTH && 
               password.length() <= MAX_LENGTH;
    }
    
    @Override
    public String getErrorMessage() {
        return "Password must be between " + MIN_LENGTH + " and " + MAX_LENGTH + " characters";
    }
}
```

> [!WARNING]
> When implementing an interface, you must provide public implementations for all abstract methods, or declare the class as abstract.

### Multiply Two Numbers in an Array

**Task:** Method that returns the product of two specific elements in an array.

```java
public int multiplyFirstAndLast(int[] arr) {
    if (arr == null || arr.length == 0) {
        throw new IllegalArgumentException("Array cannot be null or empty");
    }
    
    int first = arr[0];
    int last = arr[arr.length - 1];
    return first * last;
}
```

> [!TIP]
> Always validate input parameters to handle edge cases like null or empty arrays.

### Count Words Using `split()`

**Task:** Split a string into words and return the word count.

```java
public int countWords(String text) {
    if (text == null || text.isEmpty()) {
        return 0;
    }
    
    // Split by one or more whitespace characters
    String[] words = text.split("\\s+");
    return words.length;
}
```

> [!EXAMPLE]
> ```java
> String sentence = "Hello   world, how are  you?";
> int count = countWords(sentence); // Returns 5
> ```

> [!NOTE]
> The regex `\\s+` matches one or more whitespace characters, handling multiple spaces between words correctly.

#java #study-guide #examples
