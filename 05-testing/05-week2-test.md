---
tags: [java, quiz, week2, practice]
date: 2025-09-04
topic: Week 2 Java Test
---


### **I. Core Java Concepts**  
#### **A. OOP Fundamentals**  
1. **Class Structure**  
   - Components: fields, methods, constructors, access modifiers.  
2. **Inheritance & Polymorphism**  
   - Extending classes (`extends`), method overriding (`@Override`), concrete vs. abstract methods.  
3. **Interfaces**  
   - Requirements for implementation (all methods must be `public`), default/static methods.  
4. **Method Overloading vs. Overriding**  
   - Overloading: same name, different parameters (compile-time).  
   - Overriding: same signature in subclass (runtime, `@Override`).  
5. **`equals()` and `hashCode()`**  
   - Contract: consistent, symmetric, transitive; `hashCode()` must be consistent with `equals()`.  

#### **B. Data Types & Memory**  
1. **Primitive Types & Pass-by-Value**  
   - `int`, `double`, `boolean`, etc.; primitives are passed by value (copied).  
2. **Heap vs. Stack Memory**  
   - **Heap**: Objects, instance variables.  
   - **Stack**: Method calls, local variables (primitives, references).  
3. **String Pool**  
   - Interned strings in heap; `==` vs `.equals()` for comparison.  

#### **C. Control Flow & Operators**  
1. **Ternary Operator**  
   - Syntax: `condition ? expr1 : expr2`.  
2. **Increment Operators**  
   - Prefix (`++i`) vs. postfix (`i++`).  
3. **Switch Statements**  
   - `switch` with `case`, `break`, `default`; supports `String`, enums (Java 7+).  

#### **D. Arrays & Collections**  
1. **Arrays**  
   - Declaration, initialization, iteration; fixed size.  
2. **`String.split()`**  
   - Splits using regex delimiters (e.g., `split("\\s+")` for whitespace); counts words.  

#### **E. Exceptions**  
1. **Checked vs. Unchecked Exceptions**  
   - **Checked**: `Exception` subclasses (e.g., `IOException`); **must** be handled/declared.  
   - **Unchecked**: `RuntimeException` subclasses (e.g., `NullPointerException`); optional handling.  
2. **General Exception Handling**  
   - `try`/`catch`/`finally`, `throws` clause.  

#### **F. Advanced Topics**  
1. **Garbage Collector (GC)**  
   - **3 Key Actions**:  
     (1) Identifies unreachable objects,  
     (2) Reclaims memory,  
     (3) Compacts heap to reduce fragmentation.  
2. **Declaring Constants**  
   - `static final` variables (e.g., `public static final int MAX_SIZE = 100;`).  
3. **Parameterized Tests**  
   - JUnit 5: `@ParameterizedTest`, `@ValueSource`, `@CsvSource`.  

---

### **II. Testing (JUnit)**  
1. **Structure of a JUnit Test Case**  
   - `@Test`, assertions (`assertEquals`, `assertTrue`), setup/teardown (`@BeforeEach`, `@AfterEach`).  
2. **Creating Parameterized Tests**  
   - Annotations: `@ParameterizedTest`, data sources (`@ValueSource`, `@CsvFileSource`).  

---

### **III. Coding Questions (4 Key Tasks)**  
1. **Password Size Evaluator**  
   - **Task**: Implement logic (using `if`/`else` or ternary) to check if a `String` password is:  
     - Too short (< 8 chars),  
     - Too long (> 20 chars),  
     - Valid (8–20 chars).  
   - *Hint*: Use `password.length()`.  
2. **Implement an Interface**  
   - **Task**: Create a class that implements an interface (e.g., `Validator`).  
   - *Key*: Must define **all** abstract methods from the interface.  
3. **Multiply Two Numbers in an Array**  
   - **Task**: Method takes `int[] arr`, returns product of **two specific elements** (e.g., first and last).  
   - *Hint*: `arr[0] * arr[arr.length - 1]`.  
4. **Count Words Using `split()`**  
   - **Task**: Split a `String` into words (using regex delimiter like `\\s+`) and return word count.  
   - *Example*: `"Hello world".split("\\s+").length` → `2`.  

#java #testing #junit
