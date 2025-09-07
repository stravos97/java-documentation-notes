---
tags: [java, advanced-java-topics, java/patterns]
date: 2025-09-07
java-version: 17+
related: [[Core Java Concepts]], [[Runtime and Memory Management]]
---

# Advanced Java Topics

## Quick Reference
- **What**: Higher-level features like streams and SOLID design
- **When**: Building modular, maintainable code
- **Java Version**: 8+ (stream API), 11+ (various improvements)
- **Package**: `java.util.stream`

## Overview
Streams provide a fluent way to work with collections. SOLID principles guide object-oriented design, helping classes stay focused. Exceptions classify failures so programs can handle them predictably.

## Core Concepts

### Essential Knowledge
```java
import java.util.List;

public class StreamDemo {
    public static void main(String[] args) {
        List<String> words = List.of("one", "two", "three");
        long count = words.stream().filter(w -> w.length() > 3).count();
        System.out.println(count);
    }
}
```

### How It Works
A stream pipeline has a source, intermediate operations, and a terminal operation. SOLID's single-responsibility rule says a class should only do one job. Checked exceptions must be declared or handled, while unchecked ones can bubble up.

## Practical Implementation

### Basic Usage
```java
class Calculator {
    int add(int a, int b) { return a + b; }
}
```
Wrap this class in a strategy interface to swap implementations:
```java
interface Operation { int apply(int a, int b); }
class Add implements Operation { public int apply(int a, int b) { return a + b; } }
```

### Advanced Patterns
Use method references to simplify lambdas:
```java
List<String> names = List.of("Ann", "Bob");
names.forEach(System.out::println);
```

## Common Pitfalls
> [!WARNING] Too Many Streams
> Long stream pipelines hurt readability. Break them into smaller methods when they get complex.

## Performance Considerations
| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| Stream API | O(n) | O(n) | Declarative processing |
| Loops | O(n) | O(1) | Simple tasks |

## Related Concepts
- [[Core Java Concepts]]
- [[Runtime and Memory Management]]
- #java/patterns

## Quick Examples Repository
<details>
<summary>Click for more examples</summary>

```java
try (var lines = Files.lines(Path.of("data.txt"))) {
    lines.forEach(System.out::println);
}
```
</details>
