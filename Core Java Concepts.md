---
tags: [java, core-java-concepts, java/core]
date: 2025-09-07
java-version: 17+
related: [[Advanced Java Topics]], [[Runtime and Memory Management]]
---

# Core Java Concepts

## Quick Reference
- **What**: Foundation of the language—syntax, objects, and collections
- **When**: Building simple programs or scaffolding larger systems
- **Java Version**: 8+ (examples use 17)
- **Package**: `java.base`

## Overview
Java starts with classes and the `main` method. Types are split between primitives and objects. Collections such as `List` and `Map` manage groups of data. Understanding these basics sets the stage for every other feature.

## Core Concepts

### Essential Knowledge
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java");
    }
}
```

### How It Works
The JVM looks for `public static void main(String[] args)` as the entry point. Primitives like `int` live on the stack, while objects live on the heap. Collections in `java.util` offer dynamic storage; `ArrayList` is common for indexed access.

## Practical Implementation

### Basic Usage
```java
import java.util.ArrayList;
import java.util.List;

public class NumbersExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        for (int n : numbers) {
            System.out.println(n);
        }
    }
}
```

### Advanced Patterns
Use a simple factory when object creation needs a small decision:
```java
class ShapeFactory {
    static Shape create(String type) {
        return switch (type) {
            case "circle" -> new Circle();
            case "square" -> new Square();
            default -> throw new IllegalArgumentException("Unknown type");
        };
    }
}
```

## Common Pitfalls
> [!WARNING] Gotcha
> `NullPointerException` appears when calling methods on `null`. Always check references or use `Optional`.

## Performance Considerations
| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| `ArrayList` | O(1) | O(n) | Random access |
| `LinkedList` | O(n) | O(n) | Frequent inserts |

## Related Concepts
- [[Advanced Java Topics]]
- [[Runtime and Memory Management]]
- #java/core

## Quick Examples Repository
<details>
<summary>Click for more examples</summary>

```java
String message = String.format("%s %d", "Version", 17);
```
</details>
