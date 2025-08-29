---
tags: [java, memory-management, stack, heap, garbage-collection, reference-types, cheatsheet]
date: 2025-08-29
topic: Java Memory Management and Object Lifecycle
---

# Memory Management

1. What is stored on the stack?
2. What is stored on the heap?
3. Where are strings stored in memory?
4. What are reference types vs value types?
5. What happens to objects when they go out of scope?
6. What three things does the garbage collector do?
7. How are memory addresses different for objects with the same values?

## Java Memory Model (Overview)

Java manages memory automatically, but understanding **where things live** and **how the JVM cleans up** is crucial for writing efficient, leak-free code. The main memory areas are:

- **Stack:** Fast, temporary, method-specific storage
- **Heap:** Long-lived, general-purpose object storage
- **String Constant Pool:** Special heap area for string literals
- **Method Area:** Stores class metadata, static variables, and compiled code

***

## What is Stored on the Stack?

**Stack Memory** contains:

- **Primitive values** (`int`, `double`, `boolean`, `char`, etc.)
- **Local variables** (declared inside methods or blocks)
- **Method call frames** (parameters, return address, local vars for each method call)
- **Object references** (not the objects themselves, just the memory address/pointer)

**Stack is managed via Last-In-First-Out (LIFO)**—when a method ends, its stack frame is *popped* and memory is freed **instantly**.

### Examples

```java
public void process() {
    int age = 30;                  // Primitive on stack
    double price = 19.99;          // Primitive on stack
    String name = "Alice";         // Reference on stack, object in heap
    calculateTotal(age, price);    // Method call pushes new frame
}

private void calculateTotal(int years, double cost) {
    int discount = 10;             // Primitive on stack
    double total = cost - discount;// Primitive on stack
    System.out.println(total);
}
```

> [!TIP]
> **Stack is very fast** but **limited in size**—deep recursion or large local collections can cause **StackOverflowError**.

> [!WARNING]
> **Do NOT store large data structures in local variables**—use the heap.

***

## What is Stored on the Heap?

**Heap Memory** contains:

- **All objects** (instances of classes, arrays)
- **Instance variables** (fields inside objects)
- **Static variables** (class-level, shared across instances)
- **String objects** (except literals, see below)

Heap is **managed by the garbage collector**—objects live until no longer referenced, then are cleaned up.

### Examples

```java
class Product {
    private String name;            // Field on heap (inside Product object)
    private double price;           // Field on heap (inside Product object)

    public Product(String n, double p) {
        name = n;
        price = p;
    }
}

public class Shop {
    private static List<Product> inventory = new ArrayList<>(); // Static on heap

    public void addProduct(String name, double price) {
        Product p = new Product(name, price); // Object created on heap
        inventory.add(p);                     // Reference stored in heap List
    }
}
```

**Heap is slower to access** than stack, but **can grow as needed** (up to JVM limits).

> [!INFO]
> **Heap can fill up**—if references are retained unnecessarily, you get **OutOfMemoryError**.

***

## Where Are Strings Stored?

**Strings are special** because Java tries to **reuse identical literals** for efficiency:

| Creation Method    | Storage Location            | Behavior                  |
| :----------------- | :-------------------------- | :------------------------ |
| **String Literal** | String Constant Pool (heap) | Reuses existing string    |
| **`new String()`** | Regular Heap                | Always creates new object |

Java 7+ moved the **String Constant Pool (SCP)** from a special area (**PermGen**) to the **main heap**, making it subject to garbage collection.

### Examples

```java
String s1 = "Hello";           // SCP (heap)
String s2 = "Hello";           // SCP (heap) — same object reused
String s3 = new String("Hello"); // Heap (new object)
String s4 = new String("Hello"); // Heap (another new object)

System.out.println(s1 == s2);    // true (same object)
System.out.println(s1 == s3);    // false (different objects)
System.out.println(s3 == s4);    // false (different objects)
```

> [!TIP]
> **Use string literals** when possible to save memory.
> **Avoid `new String("literal")`** unless you explicitly need a new object.

***

## Reference Types vs Value Types

| Feature        | Value Type (Primitive)     | Reference Type (Object)             |
| :------------- | :------------------------- | :---------------------------------- |
| **Storage**    | On stack                   | On heap (object), ref on stack      |
| **Copy**       | Copies value               | Copies reference (pointer)          |
| **Examples**   | `int`, `double`, `boolean` | `String`, `Object`, arrays, classes |
| **==**         | Compares values            | Compares references                 |
| **Assignment** | `a = b` copies value       | `a = b` copies reference            |

### Examples

```java
int a = 5;
int b = a;      // Copies value (5)
b = 10;         // a still 5

List<String> listA = new ArrayList<>();
listA.add("Alice");
List<String> listB = listA;    // Copies reference
listB.add("Bob");              // Both listA and listB see the change
System.out.println(listA == listB); // true (same object)
```

> [!WARNING]
> **Mixing `==` and `.equals()`**:
> For objects, **`==` checks memory address**, **`.equals()` checks content**.
> For primitives, **`==` is always value equality**.

***

## What Happens When Objects Go Out of Scope?

When a **reference goes out of scope** (e.g., method ends, local variable dies), the **object becomes unreachable** and **eligible for garbage collection**. The JVM will **eventually reclaim its memory**.

### Ways Objects Become Eligible for GC

- **Reference set to null:** `obj = null;`
- **Reference reassigned:** `obj = new Object(); // old object now eligible`
- **Method exits:** Local references disappear, objects may become unreachable.
- **Island of isolation:** Circular references with no external access.

### Example

```java
public void demoScope() {
    List<Integer> numbers = new ArrayList<>();
    numbers.add(1); numbers.add(2); numbers.add(3);
    // numbers holds a reference to the ArrayList in heap
    processNumbers(numbers);
    // After processNumbers completes, numbers is out of scope
    // The ArrayList becomes eligible for GC
}
```

**Memory is not immediately freed**—GC runs **when the JVM decides** (based on memory pressure).

***

## Garbage Collector: What Does It Do?

The **Garbage Collector (GC)** has three main jobs:

| Step        | What Happens                                         | Purpose                    |
| :---------- | :--------------------------------------------------- | :------------------------- |
| **Mark**    | Finds all reachable objects (starting from GC roots) | Identify live objects      |
| **Sweep**   | Deletes unreachable objects                          | Free memory                |
| **Compact** | Moves objects to reduce fragmentation                | Improve future allocations |

### GC Roots

- **Active threads**
- **Static variables**
- **Local variables in stack frames**
- **JNI references**

### Example

```java
public class GCExample {
    public static void main(String[] args) {
        Object a = new Object();      // Created, referenced by 'a'
        Object b = new Object();      // Created, referenced by 'b'
        a = b;                        // First object now unreachable
                                      // Eligible for GC
    }
}
```

> [!WARNING]
> **`System.gc()` is a request**—do **not rely on it** for memory management.
> The JVM decides **when and if** to run the GC.

***

## Memory Addresses and Object Identity

- **Every `new` creates a new object with a unique memory address**.
- **Objects with the same contents are still distinct** unless explicitly pooled (like string literals).
- **`==` compares addresses**, **`.equals()` compares contents**.

### Example

```java
String a = new String("Hello");
String b = new String("Hello");
System.out.println(a == b);           // false (different objects)
System.out.println(a.equals(b));      // true (same content)
System.out.println(System.identityHashCode(a)); // Unique for each object
System.out.println(System.identityHashCode(b)); // Unique for each object
```

***

## Visual Memory Model

Here’s a **simplified view** of how memory is organized at runtime:

```
STACK                                    HEAP
├─ int count = 5                     ├─ ArrayList<String> (inventory)
├─ String name = [ref]  ───────────> │  └─ "Banana", "Apple", "Orange"
├─ Product p = [ref]    ───────────> │  └─ Product(name="Laptop", price=999.99)
│                                     ├─ String Pool
│                                     │  └─ "Alice" (literal)
├─ method frames (process, main)      │  └─ "Hello" (literal)
│                                     └─ Method Area
│                                        └─ static int totalSales = 1000
```

- **Stack**: Primitives, references, method frames
- **Heap**: Objects, arrays, instance/static fields
- **String Pool**: Cached string literals
- **Method Area**: Class metadata, static variables

***

## Garbage Collection Algorithms

| GC Type      | Use Case                    | Pause Time | Throughput | Notes                   |
| :----------- | :-------------------------- | :--------- | :--------- | :---------------------- |
| **Serial**   | Small, single-core apps     | High       | Low        | Simple, single-threaded |
| **Parallel** | Batch, multi-core           | Medium     | High       | Uses multiple threads   |
| **CMS**      | Low-latency, interactive    | Low        | Medium     | Concurrent, phased      |
| **G1**       | Large heaps, balanced       | Very Low   | High       | Default in modern Java  |
| **ZGC**      | Huge heaps, ultra-low pause | Ultra-Low  | High       | New, for massive apps   |

***

## Quick Reference Table

| Memory Area     | Contains                        | Cleanup Method          | Access Speed  | Max Size?        |
| :-------------- | :------------------------------ | :---------------------- | :------------ | :--------------- |
| **Stack**       | Primitives, refs, method frames | Automatic (method exit) | Very Fast     | Limited (by JVM) |
| **Heap**        | Objects, arrays, fields         | Garbage Collection      | Slower        | Up to JVM max    |
| **String Pool** | String literals                 | Garbage Collection      | Fast (cached) | Part of heap     |
| **Method Area** | Static fields, class metadata   | Garbage Collection      | Fast          | Part of heap     |

***

## Best Practices and Common Pitfalls

> [!TIP]
> - **Prefer local variables** for small, short-lived data.
> - **Avoid holding references** to objects longer than necessary—this can cause memory leaks.
> - **Use `try-with-resources`** for closable resources (files, sockets) to prevent leaks.
> - **String literals** are memory-efficient; **`new String("...")`** is usually unnecessary.
> - **Profile memory usage** with tools like VisualVM if your app is memory-heavy.

> [!WARNING]
> - **Static collections** that grow without bounds are a classic memory leak.
> - **Event listeners** that aren’t properly removed can leak memory.
> - **Deep recursion** can cause **StackOverflowError**.
> - **Large local collections** can exhaust stack memory—use the heap.

> [!EXAMPLE]
> A **real-world leak pattern**:
> ```java
> public class Cache {
>     private static Map<Long, Product> inventory = new HashMap<>();
>     public static void addProduct(Product p) {
>         inventory.put(p.getId(), p); // Products never removed!
>     }
> }
> ```
> **Solution:** Implement a size limit or cache eviction policy.

***

## Common Memory-Related Errors

- **StackOverflowError**: Too deep recursion, too many method calls, large local collections.
- **OutOfMemoryError**: Heap exhausted—too many objects retained, memory leaks.
- **PermGen/Metaspace Error**: In older JVMs, too many classes loaded (modern JVMs use Metaspace on heap).

***

## Tags

#java #memory #stack #heap #garbage-collection #string-pool #reference-types #value-types #bestpractices #cheatsheet

***

## Related Topics

- \[[Java Performance Tuning]\]
- \[[How the JVM Works]\]
- \[[Java Collections Memory Impact]\]
- \[[Effective Resource Management]\]
