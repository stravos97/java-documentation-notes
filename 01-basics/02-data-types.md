---
tags: [java, data-types, wrapper-classes, enums, methods, strings, arrays, cheatsheet]
date: 2025-08-29
topic: Java Data Types, Wrapper Classes, and Basic Concepts
---

# Data Types and Classes

1. What are the Integer and Boolean wrapper classes?
2. When and how do you use the var keyword?
3. Strings and Arrays
4. Are strings and arrays mutable or immutable?
5. How do you use string split and substring?
6. How can regex be used as a delimiter?
7. Enums
8. What are enums and when are they used?
9. Why are enums similar to final constants?
10. What is the naming convention for final constants?
11. Object Creation
12. How can you create objects without using the new keyword?
13. Methods
14. What are method interfaces?
15. What is method overloading?
16. What is method overriding?
17. Can the main method be overloaded?
18. Why should methods only return values and not contain System.out.println?

***

## Data Types and Classes

### Integer and Boolean Wrapper Classes (Expanded)

**Integer** wraps the primitive `int` so you can use an integer where objects are needed—for example, in [Java Collections](Java%20Collections) like `List<Integer>`. It also provides useful static methods (`parseInt`, `toString`, `max`, `min`, `sum`) and constants (`MIN_VALUE`, `MAX_VALUE`).

```java
Integer age = Integer.valueOf(17);      // Best practice: use valueOf()
int unwrapped = age.intValue();          // Get the int back

int parsed = Integer.parseInt("42");     // Convert a String to int
String numStr = Integer.toString(123);   // Convert an int to String
int bigger = Integer.max(5, 10);         // Returns 10
```

**Boolean** wraps `boolean` and is required for collections like `List<Boolean>`. It also has factory methods (`valueOf`) and static constants (`TRUE`, `FALSE`).

```java
Boolean active = Boolean.valueOf(true);  // Factory method
Boolean inactive = Boolean.FALSE;        // Use static constant
boolean b = active.booleanValue();       // Unbox the boolean
Boolean parsed = Boolean.valueOf("true");// Parse from string
```

> [!TIP]
> Use `valueOf` instead of the constructor (e.g., `new Integer(5)`)—it’s more efficient and is the recommended way in modern Java.
>
> Avoid using wrapper classes unless you need their special features—primitives are more efficient for simple variables.

> [!WARNING]
> **Autoboxing/Unboxing** can slow down performance in loops (Java automatically converts between primitives and wrappers).
>
> **NullPointerException**: Wrappers can be `null`; primitives can’t.

***

## Using the `var` Keyword (Expanded)

**`var`** allows Java to infer the variable’s type from its initializer (local variable only). Introduced in Java 10.

```java
var message = "Hello, world";           // Inferred as String
var primes = List.of(2, 3, 5, 7);       // Inferred as List<Integer>
var map = Map.of("key", 1);             // Inferred as Map<String, Integer>
```

> [!TIP]
> Use `var` for verbose generic types or when the type is obvious from the right side.
>
> Don’t use `var` if the type is unclear—readability is more important than brevity!

> [!WARNING]
> **`var` cannot be used for field declarations, method parameters, or return types.**
>
> **You must provide an initializer:**
> `var x; // Error – cannot use 'var' on variable without initializer`

***

## Strings and Arrays (Expanded)

### String Immutability (Expanded)

**Strings are immutable**: Once created, their value cannot change. Operations like `concat`, `substring`, `replace`, or `trim` return **new** objects.

```java
String name = "Java";
name = name.concat(" rules");   // name now points to a new String object: "Java rules"

String city = "London";
String sub = city.substring(0, 4);  // "Lond" (new String)
String cityUpper = city.toUpperCase();  // "LONDON" (new String)
String cityTrim = " Paris  ".trim();    // "Paris" (new String)
```

> [!WARNING]
> String modification operations can be **expensive**—use `StringBuilder` or `StringBuffer` for heavy string manipulation.

***

### Array Mutable, Fixed Size (Expanded)

**Arrays are mutable** and **fixed in size**. You can change the contents, but not the length or type.

```java
int[] scores = {90, 85, 78, 95};
scores[1] = 88;                          // Change the second value
// scores = new int[10];                 // You can assign a new array
// scores[4] = 100;                      // Error: ArrayIndexOutOfBoundsException

String[] pets = new String[4];
pets[0] = "cat";
pets[1] = "dog";                         // Rest are null (reference types)
pets[3] = "fish";
pets[2] = "bird";
// Pets: ["cat", "dog", "bird", "fish"]
```

> [!TIP]
> Use `Arrays.asList()` or `List.of()` to wrap arrays for use with Collections methods.

***

### String `split()` and `substring()` (Expanded)

- **`split(String regex)`**: Splits a string into an array using the regex as a delimiter.
- **`substring(int, int)`**: Returns a portion of the string from start (inclusive) to end (exclusive).
- **`substring(int)`**: Returns from start to the end.

```java
String csv = "apple,banana,cherry,date";
String[] fruits = csv.split(",");        // ["apple", "banana", "cherry", "date"]
String banana = csv.substring(6, 12);    // "banana"
String rest = csv.substring(7);          // "banana,cherry,date"

String names = "Alice Bob Charlie";
String[] parts = names.split(" ");       // ["Alice", "Bob", "Charlie"]
String charlie = names.substring(10);    // "Charlie"
```

> [!INFO]
> **`split()`** can be used with complex regular expressions.
> **`substring()`** is used for text extraction.

***

### Using Regex as Delimiter (Expanded)

Regular expressions let you split using multiple or complex delimiters.

```java
String data = "apple,banana;cherry orange";
String[] items = data.split("[ ,;]");    // Split by comma, space, or semicolon

String mixed = "abc123def456xyz";
String[] letters = mixed.split("\\d+");  // Split by one or more digits: ["abc", "def", "xyz"]
```

> [!TIP]
> **`[]`** in regex matches any character inside it.
> **`\\d`** matches digits; **`+`** means “one or more.”

***

## Enums (Expanded)

### Enum Definition and Usage

**Enums** are type-safe, predefined sets of constants. Use them for fixed, meaningful values.

```java
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }

Day today = Day.MONDAY;

// Method example: using enums in a switch
switch(today) {
    case MONDAY -> System.out.println("Start of the week!");
    case FRIDAY -> System.out.println("Almost weekend!");
    default -> System.out.println("Another day...");
}

// Iterate all values
for (Day d : Day.values()) {
    System.out.println(d);
}
```


***

### Enums with Fields and Methods (Examples)

Enums can have properties and methods.

```java
enum Planet {
    EARTH(6371), MARS(3390), VENUS(6052);
    private final int radius;  // in kilometers
    Planet(int r) { radius = r; }
    public int getRadius() { return radius; }
}

Planet earth = Planet.EARTH;
System.out.println(earth.getRadius());  // 6371
```

> [!EXAMPLE]
> Enums are like classes—you can add fields, constructors, and methods.

***

### Enums vs. Final Constants

Using enums is **much better** than a bunch of `static final` constants or literals. Enums are **type-safe** and support **iteration**.

```java
// Old way (not recommended)
public static final int MONDAY = 0;
public static final int TUESDAY = 1;
...
// enum (recommended)
enum Day { MONDAY, TUESDAY, ... }
```


***

### Naming Convention for Final Constants

- **Constant** (`static final`) fields: `ALL_CAPS_WITH_UNDERSCORES`.
- **Enum** names and values: `UPPER_CASE_WITH_UNDERSCORES`.
- **Classes, enums, interfaces**: `PascalCase`.

```java
// Constants
public static final int MAX_VALUE = 100;
// Enums
enum HttpStatus { OK, NOT_FOUND, SERVER_ERROR }
```


***

## Object Creation (Expanded)

You can create objects in Java **without using `new`** in several ways:

- **Factory methods**: `Integer.valueOf(5)`, `Boolean.valueOf(true)`, `String.valueOf(...)`, `List.of(...)`, etc.
- **Reflection**: `Class.forName(...).getConstructor().newInstance()` (advanced; usually not required).
- **Deserialization**: Reading objects from files or streams (`ObjectInputStream.readObject()`).
- **Cloning**: `object.clone()` (must implement `Cloneable`).
- **Builder pattern**: Pattern for creating complex objects.

```java
// Factory methods
Integer num = Integer.valueOf(5);
Boolean flag = Boolean.TRUE;
String str = String.valueOf(123);

// List.of (Java 9+)
List<String> colors = List.of("red", "green", "blue");

// Builder pattern (example)
public class Person {
    public static class Builder {
        private String name;
        public Builder name(String name) { this.name = name; return this; }
        public Person build() { return new Person(this); }
    }
    private final String name;
    private Person(Builder builder) { this.name = builder.name; }
}

Person p = new Person.Builder().name("Alice").build();
```

> [!TIP]
> Use **factory methods** whenever possible—they may be more efficient (`valueOf` often returns cached objects).

> [!WARNING]
> **Primitives** (e.g., `int`, `double`) are not objects and cannot be created with `new` or factory methods. They use literal syntax: `int x = 5;`

***

## Methods (Expanded)

### Method Interfaces (Functional Interfaces)

A **method interface** (functional interface) is an interface with **exactly one abstract method**. Used for **lambda expressions** and **method references**.

```java
// Built-in examples
Runnable r = () -> System.out.println("Running!");
Predicate<String> isEmpty = s -> s.isEmpty();
Comparator<Integer> compare = (a, b) -> a - b;

// Custom functional interface
@FunctionalInterface
interface Adder {
    int add(int a, int b);
}
Adder adder = (a, b) -> a + b;  // lambda
System.out.println(adder.add(3, 5));  // 8
```

> [!INFO]
> Examples of built-in functional interfaces: `Predicate`, `Consumer`, `Supplier`, `Function`, `Comparator`, `Runnable`.

***

### Method Overloading

**Overloading** means multiple methods with **the same name, but different parameters** (type, number, or order of parameters).

```java
void print(int x) { System.out.println("int: " + x); }
void print(double x) { System.out.println("double: " + x); }
void print(String s, int n) { System.out.println("String: " + s + ", int: " + n); }
void print(boolean b) { System.out.println("boolean: " + b); }

// All these are valid calls to print()
print(5);
print(3.14);
print("Hello", 42);
print(true);
```

> [!NOTE]
> **Overloading** is different from **overriding** (a subclass redefining a method from its parent).

***

### Method Overriding

**Overriding** means a subclass provides a new implementation of a method defined in its superclass.

```java
class Animal {
    void makeSound() { System.out.println("Generic animal sound"); }
}
class Cat extends Animal {
    @Override
    void makeSound() { System.out.println("Meow!"); }
}
Animal a = new Cat();
a.makeSound();  // "Meow!" – Polymorphism in action!
```

> [!TIP]
> Always use the `@Override` annotation when overriding, even though it’s not required.

***

### Can the `main` Method Be Overloaded?

Yes! But only `public static void main(String[] args)` is called by the JVM to start your program. Overloaded versions must be called explicitly.

```java
public class MainDemo {
    // Standard main called by JVM
    public static void main(String[] args) {
        System.out.println("Main method!");
        main(42);
    }
    // Overloaded main – must be called manually
    public static void main(int n) {
        System.out.println("Number: " + n);
    }
}
```

> [!WARNING]
> Overloaded `main` methods are **rarely useful**. Use a separate method if you need additional entry points.

***

### Why Should Methods Only Return Values?

**Methods should not mix logic and output**. Return values instead of printing.

#### Bad (Mixed Concerns)

```java
void addAndPrint(int a, int b) {
    System.out.println(a + b);
}
```


#### Good (Separation of Concerns)

```java
int add(int a, int b) {
    return a + b;
}

// Then, from the caller:
System.out.println(add(3, 4));
```

> [!TIP]
> **Separation of concerns** makes code more **testable**, **reusable**, and **maintainable**.

---

## Quick Reference Table (Enhanced)

| Concept             | Key Details                                      | Example Code                        |
|---------------------|--------------------------------------------------|-------------------------------------|
| Wrapper Classes     | Object versions of primitives                    | `Integer.valueOf(5)`, `Boolean.TRUE`|
| var                 | Local variable type inference (Java 10+)         | `var name = "Alice";`               |
| String immutability | All modifications create new objects             | `s = s + "!";`                      |
| Array mutability    | Elements are mutable, size is fixed              | `int[] a = {1,2,3}; a[0] = 4;`       |
| split / substring   | Split with regex, extract substrings             | `csv.split(",");`, `"abc".substr(1,2)`|
| Regex delimiter     | Use regex for complex splits                     | `split("[ ,;]")`                    |
| Enums               | Type-safe constants, can have methods            | `enum Day { MON, TUE }`             |
| Final constants     | Use ALL_CAPS, static final                       | `static final int MAX = 100;`       |
| Object w/o new      | Factory methods, reflection, deserialization     | `Integer.valueOf(5)`, `List.of()...`|
| Method overloading  | Same name, diff. params                          | `void print(int)`, `print(String)`  |
| Method overriding   | Subclass redefines parent method                 | `@Override void sound() { ... }`    |
| main overloading    | Allowed, not auto-called                         | `main(String[])` auto, `main(int)` manual |
| Return values       | Methods should return, not print                 | `int sum(int a, int b)`             |

---

## Callouts

> [!TIP]
> Use factory methods (`valueOf`, `of`, `parse...`) instead of constructors—they’re often more efficient and readable.

> [!WARNING]
> String immutability can hurt performance in loops—use `StringBuilder` or `StringBuffer` for heavy text operations.

> [!EXAMPLE]
> Enums can have methods and state:
> ```
> enum HttpStatus {
>     OK(200, "OK"),
>     NOT_FOUND(404, "Not Found");
>     HttpStatus(int code, String msg) { ... }
>     public int getCode() { ... }
>     public String getMessage() { ... }
> }
> ```

---

# Tags

#java #data-types #wrapper-classes #enums #methods #strings #arrays #bestpractices #cheatsheet #beginner

---

## Related Notes

- [Java Collections Framework](Java%20Collections%20Framework)
- [Design Patterns](Design%20Patterns)
- [Lambda and Streams](Lambda%20and%20Streams)
- [Error and Exception Handling](Error%20and%20Exception%20Handling)

#java #basics #data-types #primitives #wrappers
