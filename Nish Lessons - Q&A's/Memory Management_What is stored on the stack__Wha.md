---
tags: [java, memory-management, stack, heap, garbage-collection, reference-types, cheatsheet]
date: 2025-08-29
topic: Java Memory Management and Object Lifecycle
---

# Memory Management

What is stored on the stack?
What is stored on the heap?
Where are strings stored in memory?
What are reference types vs value types?
What happens to objects when they go out of scope?
What three things does the garbage collector do?
How are memory addresses different for objects with the same values?

## Memory Management

### What is Stored on the Stack?

**Stack memory** contains:[^1][^2][^3]

- **Primitive values** (`int`, `double`, `boolean`, etc.)
- **Local variables** (method parameters and variables declared within methods)
- **Object references** (not the objects themselves, just the memory addresses pointing to objects)
- **Method call information** (method invocation stack frames)

```java
public void exampleMethod() {
    int num = 42;              // Primitive stored on stack
    String name = "Java";      // Reference stored on stack, object in heap
    calculateSum(10, 20);      // Method call frame on stack
}
```

> [!NOTE]
> Stack memory follows **LIFO** (Last In, First Out) and is automatically cleaned up when methods complete.

***

### What is Stored on the Heap?

**Heap memory** contains:[^2][^4][^1]

- **All objects** (instances of classes)
- **Instance variables** (fields of objects)
- **Arrays** (which are objects in Java)
- **Static variables** (class-level variables)

```java
public class Student {
    private String name;        // Instance variable - stored in heap
    private int age;           // Instance variable - stored in heap
    
    public Student(String name, int age) {
        this.name = name;      // Object created in heap
        this.age = age;        // Primitive value within object in heap
    }
}

Student student = new Student("Alice", 20);  // Object stored in heap
```


***

### Where are Strings Stored in Memory?

**Strings** are stored in the **heap**, specifically in a special area called the **String Constant Pool (SCP)**:[^5][^6]

| Creation Method    | Storage Location          | Behavior                  |
|:-------------------|:--------------------------|:--------------------------|
| **String Literal** | String Pool (within heap) | Reuses existing strings   |
| **`new` Keyword**  | Heap (outside pool)       | Always creates new object |

```java
// Stored in String Constant Pool
String str1 = "Hello";
String str2 = "Hello";        // Reuses same object from pool
System.out.println(str1 == str2);  // true - same reference

// Stored in heap (outside pool)
String str3 = new String("Hello");
String str4 = new String("Hello");
System.out.println(str3 == str4);  // false - different objects
```

> [!TIP]
> String pool was moved from PermGen to heap in Java 7, improving memory management.

***

### Reference Types vs Value Types

| Aspect         | Value Types (Primitives)           | Reference Types (Objects)                  |
|:---------------|:-----------------------------------|:-------------------------------------------|
| **Storage**    | Direct value on stack              | Reference on stack, object in heap         |
| **Assignment** | Copies the actual value            | Copies the reference                       |
| **Examples**   | `int`, `double`, `boolean`, `char` | `String`, `Object`, arrays, custom classes |
| **Comparison** | `==` compares values               | `==` compares references                   |

```java
// Value types
int a = 10;
int b = a;        // Copies value
b = 20;           // a is still 10

// Reference types  
List<String> list1 = new ArrayList<>();
List<String> list2 = list1;    // Copies reference
list2.add("item");             // Both list1 and list2 see the change
```


***

### What Happens When Objects Go Out of Scope?

When objects go **out of scope**:[^7][^8]

1. **Reference becomes inaccessible** - no way to reach the object
2. **Object becomes eligible for garbage collection** - marked for cleanup
3. **Memory not immediately freed** - GC runs at JVM's discretion
4. **Eventually cleaned up** by garbage collector
```java
public void method() {
    String localStr = new String("temporary");  // Created in heap
    // ... use localStr
}  // localStr reference goes out of scope, object eligible for GC
```

**Four ways objects become eligible for GC**:[^7]

- **Nullifying reference**: `obj = null`
- **Reassigning reference**: `obj = new Object()`
- **Local objects**: When method completes
- **Island of isolation**: Circular references with no external access

***

### Three Things the Garbage Collector Does

The **Garbage Collector** performs three main functions:[^9][^7]

| Function       | Description                                         | Purpose                                          |
|:---------------|:----------------------------------------------------|:-------------------------------------------------|
| **1. Mark**    | Identifies which objects are still reachable/in use | Determines what can be cleaned up                |
| **2. Sweep**   | Removes unreachable objects from memory             | Frees up memory space                            |
| **3. Compact** | Defragments memory by moving objects together       | Reduces fragmentation, improves allocation speed |

```java
// Example of GC eligibility
public void gcExample() {
    Object obj1 = new Object();    // Created
    Object obj2 = new Object();    // Created
    
    obj1 = null;                   // obj1 eligible for GC (marked)
    obj2 = new Object();           // Previous obj2 eligible for GC
    
    System.gc();                   // Request GC (sweep & compact)
}
```

> [!WARNING]
> `System.gc()` only **requests** garbage collection - the JVM may ignore it.

***

### Memory Addresses for Objects with Same Values

**Different objects have different memory addresses**, even with identical values:[^10]

```java
String str1 = new String("Hello");
String str2 = new String("Hello");

System.out.println(str1.equals(str2));        // true - same content
System.out.println(str1 == str2);             // false - different addresses
System.out.println(System.identityHashCode(str1));  // Different hash
System.out.println(System.identityHashCode(str2));  // Different hash
```

**Key Points**:

- **Each `new` operation** creates a distinct object with unique memory address
- **Same values ≠ same memory location** (unless using string pool)
- **Identity vs Equality**: Use `==` for identity, `equals()` for content comparison

***

## Memory Layout Visualization

```java
public class MemoryExample {
    static int staticVar = 100;           // Heap (method area)
    
    public void demonstrate() {
        int stackVar = 50;                // Stack
        String poolStr = "Constant";      // Reference on stack → String pool
        String heapStr = new String("New"); // Reference on stack → Heap object
        
        Object obj = new Object();        // Reference on stack → Heap object
    }
}
```

**Memory Layout**:

```
STACK                    HEAP
├─ stackVar: 50         ├─ String Pool
├─ poolStr: [ref]  ──→  │  └─ "Constant"
├─ heapStr: [ref]  ──→  ├─ Regular Heap
├─ obj: [ref]      ──→  │  ├─ String("New")
                        │  └─ Object()
                        └─ Method Area
                           └─ staticVar: 100
```


***

## Quick Reference Table

| Memory Area     | Contains                              | Cleanup Method                | Access Speed  |
|:----------------|:--------------------------------------|:------------------------------|:--------------|
| **Stack**       | Primitives, references, method frames | Automatic (method completion) | Very Fast     |
| **Heap**        | Objects, arrays, instance variables   | Garbage Collection            | Slower        |
| **String Pool** | String literals                       | Garbage Collection            | Fast (cached) |
| **Method Area** | Static variables, class metadata      | Garbage Collection            | Fast          |

***

## Garbage Collection Types

| GC Type      | Use Case                          | Pause Time | Throughput |
|:-------------|:----------------------------------|:-----------|:-----------|
| **Serial**   | Small, single-threaded apps       | High       | Low        |
| **Parallel** | Multi-core, batch processing      | Medium     | High       |
| **CMS**      | Low-latency, user-facing apps     | Low        | Medium     |
| **G1**       | Large heaps, balanced performance | Very Low   | High       |

***

> [!TIP] Memory Optimization
> - Use string literals instead of `new String()` when possible
> - Nullify references when objects no longer needed
> - Avoid memory leaks with static collections and listeners

> [!WARNING] Common Pitfalls
> - Stack overflow from deep recursion (stack full)
> - OutOfMemoryError from heap exhaustion
> - Memory leaks from unclosed resources

> [!EXAMPLE] Complete Example
> ```java
> public class MemoryDemo {
>     private static List<String> cache = new ArrayList<>();  // Heap
> 
>     public void processData(int count) {              // count on stack
>         StringBuilder sb = new StringBuilder();       // sb reference on stack, object in heap
> 
>         for (int i = 0; i < count; i++) {            // i on stack
>             String data = "Item" + i;                 // String pool or heap
>             sb.append(data);                          // Modifies heap object
>             cache.add(data);                          // Adds to heap collection
>         }
> 
>         String result = sb.toString();                // New string in heap
>         System.out.println(result);
> 
>         // Local variables (count, sb, i, data, result) become eligible for GC
>         // when method completes, but cache retains references to strings
>     }
> }
> ```

***

\#java \#memory-management \#stack \#heap \#garbage-collection \#string-pool \#reference-types \#object-lifecycle \#bestpractices

⁂

[^1]: https://www.baeldung.com/java-stack-heap

[^2]: https://www.digitalocean.com/community/tutorials/java-heap-space-vs-stack-memory

[^3]: https://zerotomastery.io/blog/stack-memory-in-java-beginners-guide/

[^4]: https://docs.azul.com/prime/zst/java-heap-memory

[^5]: https://dev.to/soumya_deypersevere08_/memory-allocation-of-strings-in-java-1393

[^6]: https://www.geeksforgeeks.org/java/storage-of-string-in-java/

[^7]: https://www.geeksforgeeks.org/java/garbage-collection-in-java/

[^8]: https://www.codekru.com/java/object-life-cycle-in-java

[^9]: https://newrelic.com/blog/best-practices/java-garbage-collection

[^10]: https://stackoverflow.com/questions/71797881/are-object-variables-with-the-same-address-always-identical-in-javascript

[^11]: https://stackoverflow.com/questions/68467723/display-available-stack-memory-in-java

[^12]: https://devcenter.heroku.com/articles/java-memory-issues

[^13]: https://en.wikipedia.org/wiki/Value_type_and_reference_type

[^14]: https://www.nv5geospatialsoftware.com/docs/The_Object_Lifecycle_1.html

[^15]: https://learn.microsoft.com/en-us/dotnet/visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types

[^16]: https://www.reddit.com/r/cpp_questions/comments/tpmlob/new_object_same_memory_address/

