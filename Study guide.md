---
tags: [java, study-guide, overview, fundamentals]
date: 2025-09-04
topic: Java Core Concepts Study Guide
---


### **I. Core Java Concepts**

#### **A. OOP Fundamentals**

1. **Class Structure**
    
    - **Components: fields, methods, constructors, access modifiers.** A `class` acts as a blueprint or template for creating objects. Every standalone Java program must have at least one class.
        - **Fields (Attributes)**: These represent the state or data of each object. They are typically declared as `private` to protect the data from outside access, a principle known as `encapsulation`.
        - **Methods**: These define the behaviours or actions that objects can perform.
        - **Constructors**: A special type of method used to initialize new objects. It shares the same name as the class and has no explicit return type. A class can have multiple constructors with different parameter lists, which is known as `constructor overloading`, offering flexible ways to create objects. The `this` keyword is used within a constructor to refer to the current object and distinguish between class attributes and constructor parameters when they have the same name.
        - **Access Modifiers**: These keywords control the visibility and accessibility of classes, fields, and methods.
            - `public`: Accessible from anywhere, including the Java Virtual Machine (JVM) and other packages.
            - `private`: Accessible only within the same class, serving to hide internal state and enforce encapsulation.
            - `protected`: Accessible within the same package and by subclasses, even if those subclasses are in different packages.
            - `default` (package-private): If no modifier is specified, it's accessible only within the same package.
        - **Keywords**: The `final` keyword prevents a variable, method, or class from being changed or overridden. When declaring constants, the `static final` keywords are used, and they follow the naming convention of `ALL_CAPS_WITH_UNDERSCORES`. `static` members belong to the class itself, not to any single object, meaning they can be accessed without creating an object.
    - **Sources**: `Multiple Classes & Methods.md`, `Keywords (static).md`, `OOP Four Pillars.md`, `Class Names.md`, `Constructor Overloading.md`, `Data Types.md`, `Static Methods part 2.md`.
2. **Inheritance & Polymorphism**
    
    - **Extending classes (`extends`), method overriding (`@Override`), concrete vs. abstract methods.**
        - **Inheritance**: This OOP principle involves creating a new class (subclass/child) that is a modified version of an existing class (superclass/parent). In Java, a class can `extend` only one parent class directly, a concept known as `single inheritance`. This prevents the "diamond problem" of ambiguity that can arise with multiple class inheritance. The `final` keyword can be applied to a method to prevent it from being overridden by subclasses, or to a class to prevent it from being extended entirely.
        - **Polymorphism**: Defined as the ability of a reference to behave differently based on the runtime object it points to. It allows for flexible, maintainable code by enabling changes in behavior without altering the interface.
        - **Method Overriding**: This is a form of `dynamic polymorphism` or `runtime polymorphism`. It occurs when a subclass provides its own specific implementation for a method that is already defined in its superclass. The actual method called is determined at `runtime` based on the object's actual type. It is best practice to use the `@Override` annotation to explicitly indicate that a method is intended to override a superclass method, even though it's not strictly required.
        - **Abstract Classes**: These are non-instantiable base types that provide a partial implementation for related types within the same inheritance tree, modeling an "is-a" relationship (e.g., Shape → Circle). An abstract class cannot be instantiated directly; only its concrete subclasses can be created. They can define fields, constructors, concrete methods, and abstract methods.
            - **Abstract Methods**: These methods have only a signature (declaration) and no method body. Any class containing at least one abstract method must itself be declared `abstract`. Abstract methods cannot be `private`, `static`, or `final` as they are meant to be implemented and overridden by subclasses. They must be implemented by the first concrete (non-abstract) subclass.
            - **Concrete Methods**: These are complete methods with a body.
        - **Abstract Classes Implementing Interfaces**: An abstract class can implement an interface, and it can choose to implement some of the interface's methods while leaving others abstract for its subclasses to implement.
    - **Sources**: `Inheritance.md`, `OOP Four Pillars.md`, `Abstract Classes.md`, `Data Types.md`, `Compile Time vs Runtime.md`.
3. **Interfaces**
    
    - **Requirements for implementation (all methods must be `public`), default/static methods.** An `interface` models a `capability` or a "can do" contract, describing a set of behaviors a type can perform. When a class uses the `implements` keyword, it promises to fulfil this contract by providing method bodies for all the interface's abstract methods, unless the class itself is declared `abstract`.
        - **Multiple Interface Implementation**: Unlike classes, a class can `implement` multiple interfaces, allowing it to accumulate various "can do" capabilities, which is how Java safely handles multiple inheritance of type.
        - **Method Implementations**: All abstract methods in an interface must be implemented with `public` access.
        - **Default Methods (Java 8+)**: These methods have a body and allow interfaces to evolve without breaking existing implementing classes. Implementors inherit them but can override them if needed.
        - **Static Methods (Java 8+)**: These are utility methods associated with the interface itself, not with its implementing classes. They are called directly via the interface name (e.g., `InterfaceName.method()`).
        - **Private Methods (Java 9+)**: These methods can encapsulate reusable code within the interface body, primarily for use by default and static methods.
        - **Conflict Resolution**: If a class implements multiple interfaces that provide the same default method, Java has rules to resolve the ambiguity. In some cases, the class must explicitly override the method and choose which interface's default implementation to use (e.g., `InterfaceName.super.method()`).
    - **Sources**: `Interfaces.md`, `Abstract Classes.md`, `OOP Four Pillars.md`.
4. **Method Overloading vs. Overriding**
    
    - **Overloading: same name, different parameters (compile-time).**
    - **Overriding: same signature in subclass (runtime, `@Override`).**
        - **Method Overloading**: Also known as `static polymorphism`. It involves having multiple methods within the same class that share the same name but have different `parameter lists` (differing in type, number, or order of parameters). The correct overloaded method to call is chosen by the compiler at `compile time` based on the arguments provided. For example, `void print(int)` and `void print(String)`.
        - **Method Overriding**: Also known as `dynamic polymorphism` or `runtime polymorphism`. This occurs when a subclass provides a new implementation for a method already defined in its superclass. The method call is resolved at `runtime` based on the actual type of the object. The `@Override` annotation is recommended for clarity.
        - **`main` Method**: The `main` method can be overloaded, meaning you can have other methods named `main` with different parameter lists. However, only the `public static void main(String[] args)` signature is recognized and called by the JVM as the program's entry point.
    - **Sources**: `Data Types.md`, `Compile Time vs Runtime.md`, `Inheritance.md`, `OOP Four Pillars.md`, `public static void main.md`.
5. **`equals()` and `hashCode()`**
    
    - **Contract: consistent, symmetric, transitive; `hashCode()` must be consistent with `equals()`.**
        - **`equals()`**: This method, typically overridden from the `Object` class, is used to determine if two objects are logically equal based on their `contents` or `values`, rather than their memory addresses.
        - **`hashCode()`**: This method returns an integer hash code for an object. It is primarily used by hash-based collections (like `HashMap`, `HashSet`) for efficient storage and retrieval of objects.
        - **The Contract**: A crucial contract exists between `equals()` and `hashCode()`: if two objects are considered equal by the `equals()` method, then their `hashCode()` methods _must_ produce the same integer value. It is therefore essential to always `override both methods together`, ensuring they use the `same fields` for comparison to maintain consistency. Failing to adhere to this contract can lead to difficult-to-find bugs in collections.
        - **`==` Operator**: For objects, the `==` operator compares `references` (memory addresses), meaning it returns `true` only if both references point to the exact `same object` in memory. It does _not_ compare object contents. Unlike `equals()`, the `==` operator cannot be overridden in Java.
    - **Sources**: `Inheritance.md`, `== operator.md`, `Memory Management.md`.

#### **B. Data Types & Memory**

1. **Primitive Types & Pass-by-Value**
    
    - **`int`, `double`, `boolean`, etc.; primitives are passed by value (copied).**
        - **Primitive Types**: These are fundamental data types like `int`, `double`, `char`, `boolean`, `byte`, `short`, `long`, and `float`. They are `value types`, meaning they store their actual data directly in `stack memory`.
        - **Pass-by-Value**: Java is _always_ `pass-by-value`. For primitive types, when a variable is passed to a method, a `copy of its actual value` is made. Any changes made to the parameter within the method will _not_ affect the original variable in the calling scope.
        - **Comparison**: The `==` operator is always used to compare the `values` of primitive types. Primitive types cannot be `null`.
    - **Sources**: `Java Memory Management.md`, `Memory Management.md`, `Keywords (static).md`.
2. **Heap vs. Stack Memory**
    
    - **Heap: Objects, instance variables.**
    - **Stack: Method calls, local variables (primitives, references).** Java manages memory using primarily two areas: the Stack and the Heap.
        - **Stack Memory**:
            - **Storage**: Contains `primitive values`, `local variables` (declared inside methods or blocks), `method call frames` (parameters, return addresses), and `object references` (pointers to objects on the heap).
            - **Characteristics**: It's very `fast` to access due to its Last-In-First-Out (LIFO) structure. It has a `limited size` (typically 1MB by default). Memory is `automatically cleared` when a method exits and its stack frame is popped. Each thread in a Java application has its own stack.
        - **Heap Memory**:
            - **Storage**: Contains `all objects` (instances of classes), `arrays`, `instance variables` (fields inside objects), and `static variables`. String objects (except literals in the String Constant Pool before Java 7) are stored here.
            - **Characteristics**: It is `slower` to access than the stack but can grow much `larger` (configurable with `-Xmx`). Memory is managed by the `Garbage Collector` (GC); objects live until they are no longer referenced. The heap is `shared across all threads`. Static variables, while conceptually class-level, reside in the Method Area, which is considered part of the heap in modern JVMs.
    - **Sources**: `Java Memory Management.md`, `Memory Management.md`, `Static Methods part 2.md`.
3. **String Pool**
    
    - **Interned strings in heap; `==` vs `.equals()` for comparison.**
        - **String Creation and Storage**: `Strings` in Java are `objects`, instances of `java.lang.String`.
            - **String Literals**: When you create a `String` using a literal (e.g., `"hello"`), the Java Virtual Machine (JVM) first checks a special area in the heap called the `String Constant Pool (SCP)`. If an identical string already exists in the pool, the existing one is `reused`, returning a reference to it. If not, a new `String` object is created and placed in the pool.
            - **`new String()`**: When you create a `String` using the `new` keyword (e.g., `new String("hello")`), a `new object is *always* created in the regular heap memory`, regardless of whether an identical string exists in the SCP.
        - **`String` Immutability**: `String` objects are `immutable`. Once a `String` object is created, its value cannot be changed. Any operation that appears to modify a `String` (like `concat()`, `substring()`, `replace()`) actually returns a `new String object`.
        - **Comparison (`==` vs `.equals()`)**:
            - `==` operator: For `String` objects, `==` compares their `memory addresses` (references). So, `"hello" == "hello"` would return `true` because of string pooling (they point to the same object), but `new String("hello") == "hello"` would return `false` because one is a new object on the heap, and the other is from the pool.
            - `.equals()` method: This method compares the `actual content` (character sequence) of the `String` objects. So, `new String("hello").equals("hello")` would return `true`.
        - **Best Practice**: For most applications, prefer using `String literals` to leverage the SCP for `memory efficiency`.
    - **Sources**: `Memory Management.md`, `Strings.md`, `Java Memory Management.md`.

#### **C. Control Flow & Operators**

1. **Ternary Operator**
    
    - **Syntax: `condition ? expr1 : expr2`.** The `ternary operator` is a concise shortcut for simple `if-else` assignment statements.
        - **Syntax**: `condition ? expressionIfTrue : expressionIfFalse`.
        - **Usage**: It evaluates the `condition`. If `true`, `expressionIfTrue` is executed; otherwise, `expressionIfFalse` is executed. It's best used for clear, simple assignments to improve readability, but nesting or overusing it can make code harder to read.
    - **Sources**: `IDE and Project Management.md`.
2. **Increment Operators**
    
    - **Prefix (`++i`) vs. postfix (`i++`).** Java provides increment (`++`) and decrement (`--`) operators. The position of the operator affects when the increment/decrement takes place relative to the value's use in an expression.
        - **Prefix Increment (`++i`)**: The variable is `incremented first`, and then its new value is used in the expression.
        - **Postfix Increment (`i++`)**: The variable's `current value is used first` in the expression, and then it is incremented.
        - **Efficiency**: In loops, prefix increment is `slightly more efficient`, but the difference is usually negligible for most use cases. Consistency in style across a project is often more important.
    - **Sources**: `IDE and Project Management.md`.
3. **Switch Statements**
    
    - **`switch` with `case`, `break`, `default`; supports `String`, enums (Java 7+).** The `switch` statement (`switch` keyword) is used when there are `many possible values` for a single variable, allowing for multi-branch selection.
        - **Basic Structure**: It evaluates an expression, and then `case` statements (`case` keyword) match the expression's value. A `break` statement (`break` keyword) is typically used to exit the `switch` block after a match is found, preventing "fall-through". The `default` statement (`default` keyword) is an optional fallback that executes if no `case` matches.
        - **Fall-Through**: Without a `break` statement, execution will "fall through" to the next `case` block. While sometimes intentional, unintentional fall-through is a common source of bugs. It is generally considered bad practice unless explicitly documented for a specific, rare use case.
        - **Supported Types**: `switch` statements support primitive types (`byte`, `short`, `char`, `int`), their corresponding wrapper classes, `String` (since Java 7), and `enums`.
        - **Java 14+ Enhancements**: Modern Java versions (14+) introduced `switch expressions`, which can return a value and often use an `arrow (->)` syntax, implicitly handling breaks.
    - **Sources**: `IDE and Project Management.md`, `Keywords (static).md`.

#### **D. Arrays & Collections**

1. **Arrays**
    
    - **Declaration, initialization, iteration; fixed size.**
        - **Definition**: Arrays are container objects that hold a `fixed number` of values of a single type.
        - **Mutability**: The `contents` of an array are `mutable` (can be changed), but its `length` and `type` are `fixed` once created.
        - **Memory**: Arrays are `reference types` and are stored on the `heap`.
        - **Indexing**: Java arrays are `zero-indexed`, meaning the first element is at index `0`, and the last element is at index `length - 1`. Accessing an index outside this range will result in an `ArrayIndexOutOfBoundsException`.
        - **Iteration**: When looping through an array, the condition `i < arr.length` is used to process all elements from `0` to `length - 1`. The condition `i < arr.length - 1` is used when you need to compare `adjacent elements` and want to stop before the very last element.
    - **Sources**: `Data Types.md`, `IDE and Project Management.md`, `Java Memory Management.md`.
2. **`String.split()`**
    
    - **Splits using regex delimiters (e.g., `split("\\s+")` for whitespace); counts words.**
        - **Purpose**: The `split(String regex)` method of the `String` class is used to divide a string into an `array of substrings` based on a specified `regular expression` as a delimiter. For example, `String.split("\\s+")` splits a string by one or more whitespace characters.
        - **Example Usage**: To count words in a sentence, you can split the string by whitespace and then get the `length` of the resulting array. For instance, `"Hello world".split("\\s+").length` would result in `2`.
        - **Regular Expressions**: Regular expressions (regex) provide powerful and flexible ways to define complex delimiters for splitting strings, allowing for patterns like `"[ ,;]"` to split by space, comma, or semicolon.
    - **Sources**: `Data Types.md`, `Strings.md`, `Concise Java and JUnit Study Guide`.

#### **E. Exceptions**

1. **Checked vs. Unchecked Exceptions**
    
    - **Checked: `Exception` subclasses (e.g., `IOException`); must be handled/declared.**
    - **Unchecked: `RuntimeException` subclasses (e.g., `NullPointerException`); optional handling.** Exceptions are messages that indicate when something unexpected happens during program execution, helping to manage and recover from errors.
        - **Checked Exceptions**:
            - **Definition**: These are subclasses of `Exception` (but not `RuntimeException`). Examples include `IOException`, `FileNotFoundException`, and `SQLException`.
            - **Handling Requirement**: Java `requires` these exceptions to be `handled` (caught in a `try-catch` block) or `declared` (using a `throws` clause in the method signature). If you don't handle or declare a checked exception, your code `will not compile`.
            - **Use Cases**: Used for `recoverable errors` that are typically outside the immediate control of the program's logic, such as issues with file I/O or network connections.
        - **Unchecked Exceptions**:
            - **Definition**: These are subclasses of `RuntimeException`. Examples include `NullPointerException`, `ArrayIndexOutOfBoundsException`, and `ArithmeticException`.
            - **Handling Requirement**: Java does `not require` these exceptions to be handled or declared for the code to compile.
            - **Use Cases**: They are typically caused by `bugs` in the programmer's code, such as attempting to access an object via a `null` reference, dividing by zero, or accessing an array out of its bounds. The best practice is to `fix the underlying bug` rather than relying on catching these exceptions.
        - **Hierarchy**: All exceptions inherit from `Throwable`, which branches into `Error` (system-level, unrecoverable) and `Exception` (recoverable). `RuntimeException` is a subclass of `Exception`.
    - **Sources**: `Checked and Unchecked Exceptions.md`, `Exceptions Examples.md`, `Compile Time vs Runtime.md`.
2. **General Exception Handling**
    
    - **`try`/`catch`/`finally`, `throws` clause.** Java provides structured mechanisms to handle exceptions gracefully, preventing programs from crashing unexpectedly.
        - **`try` block**: Contains the code that might throw an exception.
        - **`catch` block**: Follows a `try` block and specifies the type of exception it can handle. If an exception of that type (or a subclass) occurs in the `try` block, the `catch` block's code is executed.
            - **Multi-catch**: You can have multiple `catch` blocks to handle different exception types. The order matters: `most specific exceptions should be caught first`, followed by more general ones, to avoid "unreachable code" compilation errors.
        - **`finally` block**: This block always executes, regardless of whether an exception was thrown or caught in the `try` or `catch` blocks. It's ideal for `cleaning up resources` (e.g., closing files, database connections) to prevent resource leaks.
        - **`throws` clause**: Used in a method signature to `declare` that the method might throw one or more specified `checked exceptions`. This shifts the responsibility of handling the exception to the caller of the method.
        - **`throw` keyword**: Used to `explicitly throw` an exception from within your code.
    - **Sources**: `Checked and Unchecked Exceptions.md`, `Exceptions Examples.md`, `Keywords (static).md`.

#### **F. Advanced Topics**

1. **Garbage Collector (GC)**
    
    - **3 Key Actions: (1) Identifies unreachable objects, (2) Reclaims memory, (3) Compacts heap to reduce fragmentation.** The `Garbage Collector (GC)` is Java's automatic process for managing `heap memory`. Its primary goal is to reclaim memory occupied by objects that are no longer reachable by the program, preventing memory leaks and managing heap space efficiently.
        - **1. Mark Phase (Identify Dead Objects)**: The GC scans the heap starting from `GC roots` (e.g., stack variables, static variables, active threads) to trace and `identify all reachable ("live") objects`. Any object not reachable from a GC root is considered "dead".
        - **2. Sweep Phase (Reclaim Memory)**: After marking, the GC deallocates the memory occupied by the `unmarked ("dead") objects`. This memory is then made available for future object allocations.
        - **3. Compact Phase (Defragment Heap)**: Optionally, the GC may move the `remaining "live" objects` closer together to `eliminate fragmentation` within the heap. This creates larger contiguous blocks of free memory, improving future allocation performance and cache locality.
        - **Triggering GC**: The JVM decides `when` to run GC, typically when memory is running low. It is an `automatic and periodic` process, not directly controlled by the programmer. While `System.gc()` is a hint, there's no guarantee the GC will run immediately.
        - **Eligibility for GC**: An object becomes eligible for GC when it is `unreachable` (i.e., no active variables or objects refer to it). This can happen when a reference is set to `null`, a reference is `reassigned` to another object, or local references go `out of scope` (e.g., when a method ends). Only `heap-allocated objects` (reference types) are managed by GC; stack-allocated variables are cleared automatically when their scope ends.
    - **Sources**: `Java Memory Management.md`, `Memory Management.md`.
2. **Declaring Constants**
    
    - **`static final` variables (e.g., `public static final int MAX_SIZE = 100;`).**
        - **Keywords**: Constants in Java are declared using the `static` and `final` keywords.
            - `static`: This makes the constant belong to the `class itself`, rather than to any specific instance of the class. This means there's only one copy of the constant, shared across all objects of that class.
            - `final`: This ensures that the value of the variable, once assigned, `cannot be changed`.
        - **Naming Convention**: The standard naming convention for `static final` constants is `ALL_CAPS_WITH_UNDERSCORES` (e.g., `MAX_SIZE`, `PI`).
        - **Usage**: Constants are ideal for values that do not change throughout the program's execution, such as mathematical constants, configuration settings, or fixed limits. They are also thread-safe due to their immutability.
        - **Enums as an alternative**: While `static final` constants are useful, `enums` are often a more type-safe and powerful alternative for predefined sets of constants, especially when they need to have associated behavior or data.
    - **Sources**: `Data Types.md`, `Keywords (static).md`, `Static Methods part 2.md`.
3. **Parameterized Tests**
    
    - **JUnit 5: `@ParameterizedTest`, `@ValueSource`, `@CsvSource`.** `Parameterized tests` in JUnit 5 allow you to run the `same test method multiple times` with different sets of input data. This reduces code duplication and makes tests more efficient.
        - **`@ParameterizedTest`**: This annotation marks a method as a parameterized test. It must be used in conjunction with one or more `parameter source annotations`.
        - **`@ValueSource`**: Provides a `single source of arguments` for the parameterized test. It can be used for primitive types like `ints`, `strings`, `doubles`, and `booleans`. For example, `@ValueSource(strings = {"apple", "banana"})`.
        - **`@CsvSource`**: Provides `multiple arguments` per test invocation using `comma-separated values`. Each string in the `@CsvSource` represents a line of comma-separated values, which are then mapped to the test method's parameters. For example, `@CsvSource({"John, 30", "Jane, 25"})`.
        - **Parameter Mapping**: The parameters from the source (e.g., `@ValueSource`, `@CsvSource`) are mapped to the test method's arguments in order.
    - **Sources**: `JUNIT Study guide.md`, `JUNIT.md`.

---

### **II. Testing (JUnit)**

1. **Structure of a JUnit Test Case**
    
    - **`@Test`, assertions (`assertEquals`, `assertTrue`), setup/teardown (`@BeforeEach`, `@AfterEach`).** JUnit 5 provides a powerful framework for writing unit tests in Java.
        - **`@Test`**: This is the fundamental annotation used to mark a method as a test case. A test method annotated with `@Test` must be `void` and take `no parameters`.
        - **Test Lifecycle Annotations**: These annotations control when setup and teardown code is executed.
            - `@BeforeAll`: A `static` method annotated with `@BeforeAll` runs `once before all test methods` in the current test class. It's used for expensive, class-level setup (e.g., database connections).
            - `@AfterAll`: A `static` method annotated with `@AfterAll` runs `once after all test methods` in the current test class have completed. Used for class-level cleanup.
            - `@BeforeEach`: A non-static (`instance`) method annotated with `@BeforeEach` runs `before each individual test method`. It's ideal for setting up a fresh state for each test, ensuring test isolation.
            - `@AfterEach`: A non-static (`instance`) method annotated with `@AfterEach` runs `after each individual test method`. Used for cleaning up resources after each test.
        - **Execution Order**: The typical execution order is: `@BeforeAll` (once), Constructor (per test), `@BeforeEach` (per test), `@Test` method, `@AfterEach` (per test), `@AfterAll` (once).
        - **Assertions**: JUnit 5 provides standard assertion methods to verify expected outcomes. Common assertions include:
            - `assertEquals(expected, actual)`: Checks if two values are equal.
            - `assertTrue(condition)`: Checks if a condition is true.
            - `assertNotNull(object)`: Checks if an object is not null.
            - `assertThrows(ExceptionType.class, () -> methodCall())`: Checks if a method throws a specific exception.
        - **`@DisplayName`**: Used to provide more readable and descriptive names for test classes and methods in test reports.
        - **Best Practices**:
            - `Test Boundary Values`: Always test the edge cases of input ranges (e.g., empty arrays, single elements, first/last elements, min/max allowed values, nulls, invalid inputs) as bugs often hide there.
            - `Avoid Loops in Tests`: Loops within tests can hide failures and complicate debugging. Prefer multiple, focused test methods, parameterized tests, or data providers to test various scenarios cleanly.
            - `Test Independence`: Generally, tests should be independent and not rely on the execution order of other tests. Use `@Order` sparingly, typically for integration tests.
    - **Sources**: `JUNIT Study guide.md`, `JUNIT.md`, `IDE and Project Management.md`.
2. **Creating Parameterized Tests**
    
    - **Annotations: `@ParameterizedTest`, data sources (`@ValueSource`, `@CsvFileSource`).**
        - **`@ParameterizedTest`**: This annotation is key to running the same test logic with different input values. It must be paired with an annotation that provides the data source.
        - **Data Sources**:
            - **`@ValueSource`**: Used for tests requiring a `single parameter`. It supports primitive types like `ints`, `strings`, `doubles`, and `booleans`. For example, `@ValueSource(ints = {1, 2, 3})` would run the test method three times, once for each integer.
            - **`@CsvSource`**: Used for tests requiring `multiple parameters`. Each string value provided to `@CsvSource` represents a line of `comma-separated values`, which JUnit then parses and maps to the corresponding method parameters of the parameterized test. For example, `@CsvSource({"input1, expected1", "input2, expected2"})`.
            - `@CsvFileSource`: (Mentioned in the study guide as `@CsvFileSource`, though the notes primarily cover `@CsvSource` and `@ValueSource`) This would typically read data from a CSV file.
        - **Mapping Parameters**: The values from the data source are mapped to the parameters of the test method in the order they are declared.
    - **Sources**: `JUNIT Study guide.md`, `JUNIT.md`.

---

### **III. Coding Questions (4 Key Tasks)**

The foundational concepts required to address these coding tasks are extensively covered in your notes.

1. **Password Size Evaluator**
    
    - **Task**: Implement logic (using `if`/`else` or ternary) to check if a `String` password is: Too short (< 8 chars), Too long (> 20 chars), Valid (8–20 chars).
    - **Hint**: Use `password.length()`.
        - **Conditional Logic**: This task primarily relies on `if`/`else` statements (`if` and `else` keywords) or the `ternary operator` (`condition ? expr1 : expr2`). These constructs allow you to define different code paths based on whether the password's length meets the criteria.
        - **String Length**: The `String` class, being an `object`, provides methods to manipulate and query its contents. The `length()` method is used to obtain the number of characters in the string. You would compare the result of `password.length()` against the specified bounds (8 and 20).
    - **Sources**: `IDE and Project Management.md`, `Strings.md`, `Keywords (static).md`.
2. **Implement an Interface**
    
    - **Task**: Create a class that implements an interface (e.g., `Validator`).
    - **Key**: Must define **all** abstract methods from the interface.
        - **Interface Implementation**: To implement an `interface`, a class uses the `implements` keyword. By doing so, the class commits to providing concrete `public implementations` for `all abstract methods` declared in that interface. If the class fails to implement all abstract methods, it must itself be declared `abstract`.
        - **Contract**: Interfaces define a "can do" contract, outlining a set of behaviors without specifying their implementation. The implementing class fulfills this contract by providing the "how" for each behavior.
    - **Sources**: `Interfaces.md`, `Keywords (static).md`, `Abstract Classes.md`.
3. **Multiply Two Numbers in an Array**
    
    - **Task**: Method takes `int[] arr`, returns product of **two specific elements** (e.g., first and last).
    - **Hint**: `arr * arr[arr.length - 1]`.
        - **Arrays**: Arrays are fixed-size data structures where elements are stored in contiguous memory locations. They are `zero-indexed`, meaning the first element is at index `0` and the last is at `length - 1`.
        - **Accessing Elements**: To access the first element, you would use `arr`. To access the last element, you would use `arr[arr.length - 1]`. The `length` property provides the total number of elements in the array.
        - **Methods**: The task requires creating a `method` that takes the integer array as a parameter and returns an `integer` (the product). Methods define the behaviors objects can perform, and they can return values.
    - **Sources**: `Data Types.md`, `IDE and Project Management.md`, `Multiple Classes & Methods.md`, `Data Types.md`.
4. **Count Words Using `split()`**
    
    - **Task**: Split a `String` into words (using regex delimiter like `\\s+`) and return word count.
    - **Example**: `"Hello world".split("\\s+").length` → `2`.
        - **`String.split()` Method**: The `String` class, being an `object`, provides the `split(String regex)` method. This method takes a `regular expression` as a delimiter and returns a `String array` containing the substrings that result from splitting the original string.
        - **Regular Expression for Whitespace**: The regular expression `\\s+` is commonly used to match `one or more whitespace characters` (spaces, tabs, newlines). This is effective for splitting a sentence into individual words.
        - **Counting Words**: Once the `String` is split into an array, the `length` property of the resulting array will give you the total `word count`. For example, `"Hello world".split("\\s+").length` would produce `2`.
    - **Sources**: `Data Types.md`, `Strings.md`, `Concise Java and JUnit Study Guide`.
