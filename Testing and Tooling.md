---
tags: [java, testing-tooling, java/testing]
date: 2025-09-07
java-version: 17+
related: [[Core Java Concepts]], [[Advanced Java Topics]]
---

# Testing and Tooling

## Quick Reference
- **What**: JUnit 5 basics and common development tools
- **When**: Ensuring code correctness and managing builds
- **Java Version**: 11+ recommended
- **Package**: `org.junit.jupiter.api`

## Overview
Automated tests guard against regressions. JUnit 5 offers annotations to manage test lifecycles. Maven standardizes builds and dependencies, while IDEs accelerate development with refactoring support.

## Core Concepts

### Essential Knowledge
```java
import org.junit.jupiter.api.*;

class CalculatorTest {
    @Test
    void addsNumbers() {
        Assertions.assertEquals(4, 2 + 2);
    }
}
```

### How It Works
JUnit runs each test method in isolation. `@BeforeEach` prepares state. Maven's `pom.xml` declares dependencies so builds remain repeatable across machines.

## Practical Implementation

### Basic Usage
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertThrows;

class ExceptionTest {
    @Test
    void throwsOnNull() {
        assertThrows(NullPointerException.class, () -> List.of(null).get(0));
    }
}
```

### Advanced Patterns
```xml
<!-- pom.xml snippet -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

## Common Pitfalls
> [!WARNING] Missing Assertions
> A test without assertions doesn't verify behavior. Always check results.

## Performance Considerations
| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| Parameterized tests | O(n) | O(n) | Multiple inputs |
| Simple tests | O(1) | O(1) | Single scenario |

## Related Concepts
- [[Core Java Concepts]]
- [[Advanced Java Topics]]
- #java/testing

## Quick Examples Repository
<details>
<summary>Click for more examples</summary>

```java
// Using Maven Surefire to run tests
defaults in most IDEs
```
</details>
