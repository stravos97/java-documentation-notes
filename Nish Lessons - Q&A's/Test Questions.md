
#### **I. Core Java Syntax & OOP Fundamentals**  
**A. Basic Class Structure & OOP Pillars**  
1. What are the three fundamental components that make up a Java class?  
2. Beyond encapsulation, what are the other three main pillars of OOP supported by Java?  
3. Briefly explain what encapsulation means in OOP.  
4. What is the main purpose of a constructor in a Java class?  
5. What is the specific purpose of the `this` keyword within a constructor/method?  

**B. Inheritance & Polymorphism**  
6. How does inheritance simplify code reuse in Java?  
7. What is the main difference between an `abstract class` and an `interface`?  
8. Can a Java class inherit from multiple classes? Can it implement multiple interfaces?  
9. Why does Java support *single inheritance* for classes (not multiple inheritance)?  

**C. Access Control & Keywords**  
10. Explain the difference between `public` and `private` access modifiers.  
11. What does the `static` keyword mean for class members?  
12. What is the `final` keyword used for (e.g., with variables)?  
13. How do you declare a constant variable? What is the naming convention?  

#### **II. Memory Management & Data Types**  
**A. Memory Allocation**  
14. What is the main difference in how primitives (e.g., `int`) and objects (e.g., `String`) are stored?  
15. Briefly describe the purpose of **Stack memory**.  
16. Briefly describe the purpose of **Heap memory**.  
17. Where are `static` variables stored in modern JVMs?  

**B. String Internals**  
18. What is the Java String Pool?  
19. What does it mean for `String` objects to be "immutable"?  
20. Why is `String s = "hello";` more memory-efficient than `new String("hello")`?  
21. If `String s1 = "hello";` and `String s2 = "hello";`, what is `s1 == s2`? Explain why.  

#### **III. Object Comparison & Hashing**  
1. What is the difference between `==` and `.equals()` when comparing objects?  
2. Why must `equals()` and `hashCode()` be overridden together?  
3. Name two properties the `equals()` method must follow (e.g., Reflexive, Symmetric).  
4. Why do these methods matter in `HashMap`/`HashSet`?  
5. What utility class (`java.util.Objects`) is recommended for robust `equals()`/`hashCode()`?  
6. What happens if you use a custom object as a `HashMap` key without overriding both?  

#### **IV. Exception Handling**  
1. Key difference between **checked** and **unchecked** exceptions.  
2. When would you use a `try-catch` block?  
3. Purpose of the `finally` block.  
4. Role of the `throws` keyword in a method signature.  
5. What is the `throw` keyword used for?  
6. Primary benefit of `try-with-resources` (Java 7+)?  

#### **V. Garbage Collection**  
1. Primary function of the Java Garbage Collector (GC).  
2. When does an object become eligible for GC? (Name one scenario).  
3. Can a programmer directly control GC execution?  
4. Briefly describe the three high-level phases of GC operation.  

#### **VI. Method Mechanics**  
1. What is **method overloading**? (Simple description)  
2. What is **method overriding**? (Simple description)  
3. Difference between `++i` (prefix) and `i++` (postfix) increment?  
4. Explain "pass-by-value" for primitives passed to methods.  
5. What is the ternary operator `(condition ? a : b)` used for?  

#### **VII. Control Flow & Operators**  
1. How does a `switch` statement evaluate expressions?  
2. What happens if you omit `break` in a `case` block?  

#### **VIII. Arrays & Collections**  
1. Two key characteristics of a Java array.  
2. What does "zero-indexed" mean for arrays?  
3. How does `String.split()` divide a string?  
4. Why use `"\\."` (not `"."`) to split by a literal dot?  

#### **IX. JUnit 5 Testing**  
1. Purpose of **parameterized tests** in JUnit 5.  
2. Name one annotation for providing data to parameterized tests.  
3. What does the **Arrange-Act-Assert (AAA)** pattern refer to?  
4. How to test if a method throws a specific exception?  
5. Standard execution order of: `@BeforeAll`, `@BeforeEach`, `@Test`, `@AfterEach`, `@AfterAll`.  
6. Critical consideration for `@BeforeEach` with `static` methods?  
7. Why must JUnit tests be independent (no execution-order reliance)?  

---

### **Key Improvements Made:**  
1. **Logical Topic Grouping**  
   - Merged related concepts (e.g., all OOP topics under **I. Core Java Syntax & OOP**).  
   - Separated "basic" and "advanced" questions under subheadings (e.g., **String Internals** includes both pool basics and advanced comparisons).  

2. **Progressive Difficulty**  
   - Basic concepts first (e.g., "What is a constructor?"), followed by advanced extensions (e.g., `this` keyword, single inheritance rationale).  
   - Example: `equals()`/`hashCode()` starts with fundamentals, then dives into real-world consequences and utility classes.  

3. **Eliminated Redundancy**  
   - Removed duplicate section titles (e.g., no separate "Further Foundational Knowledge" section).  
   - Combined overlapping topics (e.g., "Memory Management" now covers both basics *and* `static` storage).  

4. **Clear Learning Path**  
   - Flow: Syntax → Memory → Objects → Exceptions → GC → Methods → Control Flow → Testing.  
   - New programmers can tackle sections in order without jumping between disconnected topics.  

5. **Practical Clarity**  
   - Grouped JUnit annotations into one cohesive testing section.  
   - Clarified string splitting (`split("\\.")` vs `split(".")`) under **Arrays & Collections** where it’s most relevant.  

This structure ensures learners build knowledge incrementally while keeping related concepts together—ideal for study guides, exams, or curriculum design.