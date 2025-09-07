---
tags: [java, spring-boot-rest-apis, java/web]
date: 2025-09-07
java-version: 17+
related: [[Core Java Concepts]], [[Testing and Tooling]]
---

# Spring Boot and REST APIs

## Quick Reference
- **What**: Rapid application setup with REST endpoints
- **When**: Building HTTP services backed by data layers
- **Java Version**: 17+
- **Package**: `org.springframework.boot`

## Overview
Spring Boot streamlines application configuration. REST controllers expose HTTP endpoints that return JSON or other media types. Repositories abstract data access so business logic stays clean.

## Core Concepts

### Essential Knowledge
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### How It Works
`@SpringBootApplication` enables component scanning and auto-configuration. Controllers annotated with `@RestController` map HTTP requests. Spring Boot starts an embedded server such as Tomcat.

## Practical Implementation

### Basic Usage
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
class HelloController {
    @GetMapping("/hello")
    String hello() { return "hi"; }
}
```

### Advanced Patterns
```java
import org.springframework.data.jpa.repository.JpaRepository;

interface UserRepository extends JpaRepository<User, Long> {}
```

## Common Pitfalls
> [!WARNING] Tight Coupling
> Putting database logic inside controllers makes code hard to maintain. Use service layers.

## Performance Considerations
| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| `@RestController` returning DTOs | O(1) | O(1) | Small responses |
| Streaming responses | O(n) | O(1) | Large datasets |

## Related Concepts
- [[Core Java Concepts]]
- [[Testing and Tooling]]
- #java/web

## Quick Examples Repository
<details>
<summary>Click for more examples</summary>

```java
// application.properties
server.port=8080
```
</details>
