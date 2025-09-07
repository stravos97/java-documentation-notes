---
tags: [java, runtime-memory-management, java/performance]
date: 2025-09-07
java-version: 17+
related: [[Core Java Concepts]], [[Advanced Java Topics]]
---

# Runtime and Memory Management

## Quick Reference
- **What**: JVM architecture, garbage collection, object lifecycle
- **When**: Tuning performance or troubleshooting
- **Java Version**: 8+ (some GC options vary)
- **Package**: `java.lang`

## Overview
Java compiles to bytecode executed on the JVM. Memory splits into stack and heap. Garbage collectors reclaim unused objects so developers can focus on logic, not manual deallocation.

## Core Concepts

### Essential Knowledge
```java
public class Counter {
    private int value;
    public void increment() { value++; }
}
```
This object lives on the heap, while method calls use stack frames.

### How It Works
The JVM class loader reads `.class` files, the verifier checks bytecode, and the JIT compiler turns hot spots into machine code. Garbage collection tracks unreachable objects; popular collectors include G1 and ZGC.

## Practical Implementation

### Basic Usage
```java
public class MemoryDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 1_000; i++) {
            new Object();
        }
        System.gc();
    }
}
```

### Advanced Patterns
Use try-with-resources to ensure timely cleanup:
```java
try (var scanner = new java.util.Scanner(System.in)) {
    System.out.println(scanner.nextLine());
}
```

## Common Pitfalls
> [!WARNING] Retained References
> Keeping references in static fields prevents garbage collection and leads to memory leaks.

## Performance Considerations
| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| G1 Collector | O(n) | O(n) | Server applications |
| Serial Collector | O(n) | O(n) | Small apps or testing |

## Related Concepts
- [[Core Java Concepts]]
- [[Advanced Java Topics]]
- #java/performance

## Quick Examples Repository
<details>
<summary>Click for more examples</summary>

```java
Runtime runtime = Runtime.getRuntime();
System.out.println(runtime.freeMemory());
```
</details>
