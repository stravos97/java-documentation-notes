
**Java Essentials: Concepts, Collections, and Concurrency - Key Questions**

**1. Core Java Class Components & OOP:**

- What are the core components of a Java class and how do they relate to Object-Oriented Programming (OOP) principles?
- What is encapsulation and how is it supported by Java class components like fields and methods?
- How do access modifiers (public, protected, private, default) control the visibility and accessibility of class members?
- What are constructors used for, and how does constructor overloading provide flexibility in object creation?
- Explain the purpose and usage of the `final` keyword in Java.
- What does the `static` keyword signify when applied to members of a class?
- When and why is the `this` keyword used within a constructor or method?
- How do abstraction, inheritance, and polymorphism relate to Java class structure and the pillars of OOP?

**2. `equals()` and `hashCode()` Contract:**

- Explain the crucial contract between the `equals()` and `hashCode()` methods in Java and why it's vital for collections.
- What is the default implementation of the `equals()` method, and when must it be overridden?
- List and describe the specific mathematical properties (`Reflexive`, `Symmetric`, `Transitive`, `Consistent`, `Non-null`) that the `equals()` method must adhere to.
- What is the primary purpose of the `hashCode()` method, and what are its key requirements?
- Why is it important for `hashCode()` to ideally produce distinct integers for unequal objects?
- What are the critical differences between the `==` operator and the `.equals()` method in Java?
- Why does this `equals()`/`hashCode()` contract matter specifically in hash-based collections (e.g., `HashMap`, `HashSet`)?
- What are the severe consequences of violating the `equals()` and `hashCode()` contract in collections, and what real-world bugs can it lead to?
- What are common implementation mistakes to avoid when overriding `equals()` and `hashCode()`?
- What best practices are recommended for implementing `equals()` and `hashCode()`, including the use of utility methods like `Objects.equals()` and `Objects.hash()`?

**3. Memory Management: Primitive Types, Objects, Heap, and Stack:**

- How do Java's primitive types and objects differ in terms of memory storage and parameter passing?
- Explain the "pass-by-value" principle in Java for both primitive types and objects.
- Describe the key differences between Stack memory and Heap memory, including what they store, their characteristics, and their lifetime.
- How do static variables relate to memory allocation in modern JVMs?

**4. String Pool & Immutability:**

- What is the Java String Pool, and why is String immutability and its handling by the pool important?
- Explain how the String Pool works when creating strings using literals versus the `new` keyword.
- Define String immutability and how operations that appear to modify a String actually work.
- Discuss the importance of String Pool and immutability in terms of memory efficiency, performance, security/thread safety, and optimized hashing.
- Why is it generally recommended to prefer string literals over `new String()`?

**5. Exception Handling:**

- What are the key distinctions between checked and unchecked exceptions in Java, and how does Java's exception handling mechanism address them?
- Provide examples of both checked and unchecked exceptions.
- Explain the `try-catch` block, `finally` block, `throws` keyword, and `throw` keyword in Java's exception handling mechanism.
- How does `try-with-resources` simplify resource management in Java 7+?
- What are best practices for using `throws` vs. `throw`, and for creating custom checked and unchecked exceptions?

**6. Garbage Collector (GC):**

- Describe the purpose and lifecycle of the Java Garbage Collector (GC) and how objects become eligible for garbage collection.
- Outline the high-level phases of the GC lifecycle (Mark, Sweep, Compact).
- What are "GC roots," and how do they determine "reachable" objects?
- What common scenarios lead to an object becoming eligible for garbage collection?
- Can developers force the GC to run immediately using `System.gc()`?
- Why should the `finalize()` method not be relied upon for critical resource cleanup?
- How can GC pauses impact application performance?

**7. Inheritance, Interfaces, & Polymorphism:**

- How does inheritance work in Java, and what are its limitations, specifically regarding multiple inheritance?
- Explain the concepts of inheritance and method overriding, and how they relate to runtime polymorphism.
- What are abstract classes and abstract methods? What rules apply to them?
- What is the difference between interfaces and abstract classes in Java?
- Describe the key features of interfaces, including default, static, and private methods introduced in Java 8 and 9+.
- How does Java resolve conflicts when a class implements multiple interfaces with conflicting default methods?

**8. Method Overloading & Overriding:**

- How do method overloading and overriding differ in Java?
- Compare overloading and overriding based on definition, parameter list, return type, access modifier, annotation usage, and resolution time.

**9. String Manipulation (`String.split()`):**

- How does the `String.split()` method work in Java, especially when dealing with regular expressions and special characters as delimiters?
- Provide examples of common regex patterns used as delimiters.
- How do you handle special characters (e.g., `.`, `|`, `*`) in regex when you want to use them as literal delimiters in `String.split()`?
- What is the purpose of the optional `limit` parameter in the `split()` method?

**10. JUnit 5 Testing:**

- How does JUnit 5 support parameterized tests, and what are the main data sources for them?
- Describe the `@ValueSource`, `@CsvSource`, and `@CsvFileSource` annotations for parameterized tests.
- What are the benefits and best practices for writing parameterized tests?
- Describe the typical execution order of annotations in a JUnit 5 test case (`@BeforeAll`, `@BeforeEach`, `@Test`, `@AfterEach`, `@AfterAll`).
- What are the best practices for JUnit testing, including boundary values, test independence, descriptive names, and the Arrange-Act-Assert (AAA) pattern?
- How does Hamcrest matchers enhance expressiveness and readability in JUnit assertions?
- Explain the modern JUnit 5 approach for exception testing using `assertThrows()`.
- What are common exam mistakes to avoid when writing JUnit tests?

**11. Miscellaneous Core Concepts:**

- How do you properly declare constants in Java, and what is the recommended naming convention?
- When are enums often a more type-safe and powerful alternative to `static final` constants?
- Explain the syntax and appropriate use of the ternary operator in Java.
- Describe the difference between prefix (`++i`) and postfix (`i++`) increment operators.
- What data types are supported by the `switch` statement in Java, and what is the importance of the `break` statement?
- What are the key characteristics of Java arrays (e.g., fixed-size, zero-indexed, memory storage)?
- What is the "System Under Test (SUT)" pattern in testing?
- When should the `@Order` annotation be used for test execution, and what is the general best practice regarding test independence?
