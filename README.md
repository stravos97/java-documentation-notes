Java Notes — Index and Conventions

This repository is organized into topic-based folders with numeric prefixes for predictable ordering. Filenames use kebab-case and clear, concise titles.

Topics
- 01-basics — Core Java fundamentals
  - [01-main-method](01-basics/01-main-method.md) — Entry point signature and purpose.
  - [02-data-types](01-basics/02-data-types.md) — Primitive vs reference types and ranges.
  - [03-operators-equality](01-basics/03-operators-equality.md) — `==` vs `equals()` and pitfalls.
  - [04-strings](01-basics/04-strings.md) — Immutability and common operations.
  - [05-while-loop](01-basics/05-while-loop.md) — While/do-while control flow examples.
  - [06-java-scanner](01-basics/06-java-scanner.md) — Reading input with `Scanner` safely.
  - [07-multiple-classes-and-methods](01-basics/07-multiple-classes-and-methods.md) — Organizing classes and methods.
  - [08-keywords-static](01-basics/08-keywords-static.md) — `static` fields/methods and use cases.
- 02-oop — Object-oriented concepts
  - [01-oop-four-pillars](02-oop/01-oop-four-pillars.md) — Encapsulation, inheritance, polymorphism, abstraction.
  - [02-inheritance](02-oop/02-inheritance.md) — `extends`, overriding, and `super` basics.
  - [03-interfaces](02-oop/03-interfaces.md) — Interfaces, default/static methods, functional interfaces.
  - [04-abstract-classes](02-oop/04-abstract-classes.md) — When to use abstract classes vs interfaces.
  - [05-constructor-overloading](02-oop/05-constructor-overloading.md) — Multiple constructors and chaining.
  - [06-class-names](02-oop/06-class-names.md) — Class naming conventions and structure.
  - [07-solid-principles](02-oop/07-solid-principles.md) — SOLID principles with practical notes.
- 03-exceptions — Error handling
  - [01-checked-vs-unchecked](03-exceptions/01-checked-vs-unchecked.md) — Differences and when to use each.
  - [02-examples](03-exceptions/02-examples.md) — try/catch/finally patterns and best practices.
- 04-streams — Functional operations
  - [01-streams-overview](04-streams/01-streams-overview.md) — Pipeline: source → intermediate → terminal.
  - [02-collection-examples](04-streams/02-collection-examples.md) — Common operations on collections.
  - [03-colon-colon-operator](04-streams/03-colon-colon-operator.md) — Method reference (`::`) patterns.
  - [04-example-problem](04-streams/04-example-problem.md) — Worked problem using streams.
- 05-testing — JUnit and testing
  - [01-junit-notes](05-testing/01-junit-notes.md) — JUnit basics, annotations, assertions.
  - [02-junit-study-guide](05-testing/02-junit-study-guide.md) — Key topics and quick reference.
  - [03-questions-and-answers](05-testing/03-questions-and-answers.md) — Personal Q&A for practice.
  - [04-test-questions](05-testing/04-test-questions.md) — Practice questions and prompts.
  - [05-week2-test](05-testing/05-week2-test.md) — Week 2 test notes.
- 07-sql — Database scripts
  - [01-mysql-northwind-database](07-sql/01-mysql-northwind-database.sql) — Base Northwind schema/data.
  - [02-mysql-northwind-database-fixed](07-sql/02-mysql-northwind-database-fixed.sql) — Fixed/updated script.
- 08-rest — RESTful APIs
  - [01-restful-apis](08-rest/01-restful-apis.md) — REST principles, resources, and HTTP methods.
  - [02-rest-api-diagrams](08-rest/02-rest-api-diagrams.md) — Visuals for API design and flows.
- 09-runtime — JVM and lifecycle
  - [01-java-virtual-machine](09-runtime/01-java-virtual-machine.md) — JVM components and execution model.
  - [02-compile-time-vs-runtime](09-runtime/02-compile-time-vs-runtime.md) — Key differences with examples.
  - [03-java-memory-management](09-runtime/03-java-memory-management.md) — Heap/stack and GC basics.
  - [04-memory-management](09-runtime/04-memory-management.md) — Extra memory tips and notes.
- 10-tools-ide — Productivity and setup
  - [01-ide-and-project-management](10-tools-ide/01-ide-and-project-management.md) — IDE setup and project hygiene.
- 11-study-guides — Consolidated prep
  - [01-study-guide-with-examples](11-study-guides/01-study-guide-with-examples.md) — Main study guide with examples.
- 12-qa — Open questions
  - [01-my-questions](12-qa/01-my-questions.md) — Topics to explore and clarify.
- 13-misc — Miscellaneous and archives
  - [01-static-methods-part-2](13-misc/01-static-methods-part-2.md) — Additional notes on static methods.
  - [02-claude](13-misc/02-claude.md) — Misc note.
- 14-spring-boot — Spring Boot and JPA
  - [01-setup](14-spring-boot/01-setup.md) — Project setup and dependencies.
  - [02-architecture-simple](14-spring-boot/02-architecture-simple.md) — Simplified app architecture.
  - [03-architecture-complex-day1](14-spring-boot/03-architecture-complex-day1.md) — Complex architecture notes.
  - [04-entities](14-spring-boot/04-entities.md) — JPA entity basics in Spring Boot.
  - [05-interfaces](14-spring-boot/05-interfaces.md) — Interfaces in a Spring Boot context.
  - [06-jpa-generated-sql-queries](14-spring-boot/06-jpa-generated-sql-queries.md) — How Spring Data JPA builds queries.
  - [07-jpa-followup-from-interfaces](14-spring-boot/07-jpa-followup-from-interfaces.md) — JPA follow-up from interfaces.
  - [08-jpa-qa-from-notes](14-spring-boot/08-jpa-qa-from-notes.md) — JPA Q&A from notes.

Conventions
- Use kebab-case for files and folders (e.g., `main-method.md`).
- Keep names short but specific; avoid punctuation like apostrophes.
- If topics overlap, scope by folder instead of duplicating names.

Tips
- Searching: use your editor’s global search to find topics quickly.
- Adding notes: place them in the most specific folder; if unsure, add to `13-misc/` and we can file them later.

Authoring Guidelines (Obsidian-friendly, no HTML)
- Clarity first
  - Favor simple, accurate explanations with progressive depth.
  - Lead with essentials; add details and edge cases after.
- Code-centric
  - Prefer modern Java (17+ LTS) and note version-specific features.
  - Provide runnable, well-commented examples that follow best practices.
  - Always format multi-line code with fenced blocks and a language hint.
    
    ```java
    // Example: Basic entry point
    public class Example {
        public static void main(String[] args) {
            System.out.println("Hello World");
        }
    }
    ```
- Structured information
  - Use headers (`##`, `###`, `####`) for folding in Obsidian.
  - Bullets for quick facts; add comparison tables where useful.
  - Include “Quick Reference” sections for syntax and common tasks.
- Practical focus
  - Explain when to use something, trade-offs, and related alternatives.
  - Include adjacent concepts (e.g., link [Java Collections](Java%20Collections) when relevant).
- No raw HTML, inline links only
  - Do not use raw HTML tags (e.g., `<details>`, `<div>`, `<span>`).
  - Use inline Markdown links like `[title](path-or-url)`; avoid reference-style links.
  - For Mermaid labels, use `\n` for line breaks instead of `<br/>`.
- Obsidian formatting
  - Callouts: use blockquotes
    - > [!NOTE] general notes
    - > [!TIP] best practices
    - > [!WARNING] pitfalls
    - > [!EXAMPLE] practical examples
    - > [!INFO] extra context
  - Tags: include relevant hashtags (e.g., `#java`, `#java/streams`).
  - Links: use double brackets for concepts (e.g., `[Design Patterns](Design%20Patterns)`).
  - Tables: use standard Markdown tables for comparisons.
- Frontmatter (optional)
  - Use YAML frontmatter for metadata when helpful.

    ---
    tags: [java, streams, bestpractices]
    date: 2025-09-07
    topic: Java Streams Quick Reference
    ---

Special instructions
- Cite sources with inline Markdown links: `[Title](https://example.com)`.
- Syntax questions: show exact syntax first, then explain.
- Error messages: explain cause and fixes with examples.
- Design patterns: include practical implementations.
- Comparisons: use concise tables or side-by-side snippets.


