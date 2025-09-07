tags: [java, topic-constructors, java/oop]
date: 2025-08-22
java-version: 17+
related: [[02-oop/01-oop-four-pillars]], [[02-oop/03-interfaces]]
topic: Java OOP, Constructors, and Access Control
---

## Why Would a Class Have Multiple Constructors?


______________________________________________________________________

<!-- Removed auto-generated TOC with broken anchors. Obsidian outline will handle navigation. -->

**Quick Reference**


- Having **multiple constructors** is called **constructor overloading**.
- It allows you to create objects in **different ways**, offering flexibility for initialization.
- Each constructor has a **different parameter list** (signature).


______________________________________________________________________

### Main Reasons to Use Multiple Constructors

#### Flexible Object Creation


- You can provide different ways to create an instance, depending on what information is available.
	- Example: Let users create a `Car` by specifying all details, only some, or none.


```java
public class Car {
    private String make;
    private String model;
    private int year;

    // No-argument constructor: set default values
    public Car() {
        this.make = "Unknown";
        this.model = "Unknown";
        this.year = 0;
    }

    // One-argument constructor
    public Car(String make) {
        this.make = make;
        this.model = "Unknown";
        this.year = 0;
    }

    // Full constructor
    public Car(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
}
```


#### Convenience and Readability


- Makes code easier to read and use, preventing the need for unnecessary or placeholder arguments.
	- E.g. `new Car("Toyota")` vs. `new Car("Toyota", "Unknown", 0)`


#### Different Initialization Logic


- Sometimes constructors perform different setup routines—a class might be initialized from a file, a database, or with
  default values.[GeeksforGeeks](https://www.geeksforgeeks.org/java/constructor-chaining-java-examples/)


#### Support for Frameworks and Tools


- Many Java frameworks (e.g., Hibernate) and tools require a **no-argument (default) constructor** for reflection or
  serialization.[boardinfinity](https://www.boardinfinity.com/blog/constructor-chaining-in-java/)[GeeksforGeeks](https://www.geeksforgeeks.org/java/constructor-chaining-java-examples/)


______________________________________________________________________

### Practical Example


| Constructor                                | Use Case                        |
|:-------------------------------------------|:--------------------------------|
| `Car()`                                    | Create a blank/default car      |
| `Car(String make)`                         | Create a car with just the make |
| `Car(String make, String model, int year)` | Full details available          |


______________________________________________________________________

### Recommended Approach


- Prefer **constructor chaining** to avoid duplicate code:


```java
public Car(String make) {
    this(make, "Unknown", 0); // calls full constructor
}

public Car() {
    this("Unknown");
}
```


- Only add constructors you need for your use cases—too many options can make code confusing.


______________________________________________________________________

### Summary


- **Multiple constructors make classes easier and safer to use in different situations**.
- This keeps code flexible, readable, and compatible with Java frameworks that might require a specific constructor
  signature.[scientecheasy](https://www.scientecheasy.com/2020/06/java-constructor-chaining.html/)[beginnersbook](https://beginnersbook.com/2013/12/java-constructor-chaining-with-example/)[boardinfinity](https://www.boardinfinity.com/blog/constructor-chaining-in-java/)


______________________________________________________________________

> [!EXAMPLE] Real-World Usage
> Provide multiple constructors when callers have different amounts of information. For example, allow a default object for quick tests and a full constructor for production data.

______________________________________________________________________

## How Does the `this` Keyword Call Another Constructor in the Same Class?

______________________________________________________________________

### Understanding Constructor Chaining with `this()`


- **`this(…)`** is a special syntax in Java for calling **another constructor in the same class** (not just referring to
  fields).
- The syntax **must appear as the first line** in the constructor.
- This allows you to reuse initialization logic, avoid code duplication, and offer flexible ways to create
  objects.[GeeksforGeeks](https://www.geeksforgeeks.org/java/constructor-chaining-java-examples/)[Baeldung](https://www.baeldung.com/java-chain-constructors)[scientecheasy](https://www.scientecheasy.com/2020/06/java-constructor-chaining.html/)[boardinfinity](https://www.boardinfinity.com/blog/constructor-chaining-in-java/)


______________________________________________________________________

### How It Works: Example


```java
public class Car {
    private String make;
    private String model;
    private int year;

    // No-argument constructor (sets default values)
    public Car() {
        this("Unknown", "Unknown", 0); // Calls the full constructor!
    }

    // Constructor with all details
    public Car(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
}
```


> [!EXAMPLE] What Happens at Runtime
> `new Car()` enters the no-arg constructor, which immediately calls the full constructor with defaults. All state is initialized in one place.

______________________________________________________________________

### Step-by-Step: What Happens


1. `new Car()` is called
1. The no-argument constructor runs first
1. The first line, `this("Unknown", "Unknown", 0);`, calls the main constructor
1. The detailed constructor sets all the fields
1. After the detailed constructor completes, control returns to the no-argument constructor—and then finishes


______________________________________________________________________

### Why Use Constructor Chaining


- **Centralizes** your initialization logic—reduces bugs and duplication
- Allows you to provide **multiple ways to create** an object (flexibility)
- Follows a "one place to initialize everything" principle, which is a good object-oriented practice[Baeldung](https://www.baeldung.com/java-chain-constructors)[dzone](https://dzone.com/articles/constructor-chaining-java-guide)[scientecheasy](https://www.scientecheasy.com/2020/06/java-constructor-chaining.html/)[boardinfinity](https://www.boardinfinity.com/blog/constructor-chaining-in-java/)[GeeksforGeeks](https://www.geeksforgeeks.org/java/constructor-chaining-java-examples/)


______________________________________________________________________

### Key Rules


- `this(…)` **must** be the first statement in the constructor (cannot appear elsewhere)
- You **cannot** call two constructors in one constructor
- There must be at least one constructor that does NOT call another constructor with `this(…)`, or you'll create an
  infinite loop


______________________________________________________________________

> [!TIP]
> Use `this(…)` in constructors when you want to chain them and avoid writing the same setup logic multiple times.

______________________________________________________________________

**Summary:**
`this(...)` calls another constructor in the same class. Use it to centralize initialization and avoid duplication (DRY).


______________________________________________________________________

## How Does `this()` Reference Another Constructor?

______________________________________________________________________

### Key Point: `this()` Is an Explicit Constructor Call


- **`this()`** is *not* just a reference—it's an *explicit call* to another constructor, chosen by its parameter list.
- When you write `this(param1, param2);` as the first line of a constructor, **Java looks for a constructor with those
  exact parameter types** in the same class and runs it.
- You never write the other constructor’s name directly: you always use `this(...)`, which links by argument types.


______________________________________________________________________

### Example


```java
public class Example {
    private int x, y;

    // 1. No-arg constructor
    public Example() {
        this(0, 0);           // Calls 2-arg constructor below
    }

    // 2. Two-arg constructor
    public Example(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```


- If you create a new object: `new Example();`
	- The zero-arg constructor runs, and the very first thing it does is `this(0, 0)`,
	- which *directly* calls the other constructor (`Example(int x, int y)`).


______________________________________________________________________

### What’s Actually Happening


- **`this(...)` = "Invoke another constructor in *this* class with these arguments."**
- Java matches the parameters you pass to the signature of the other constructor.
- You CANNOT call a constructor by its class name (e.g., `Example(1,2)` ← invalid); only by `this(...)`.


______________________________________________________________________

### Why It Looks “Invisible”


- Because you never see the constructor's name or even the word “constructor,” only the keyword `this` with parameters.
- The "magic" is: **Java connects `this(...)` to the matching constructor’s parameter list**.


______________________________________________________________________

### Rules Recap


- `this(...)` **must** be the first statement in the constructor
- You can only call one constructor per constructor via `this`
- Java chooses which one to run based on the *arguments you pass in*


______________________________________________________________________

> [!EXAMPLE] Signature Matching
> `this("foo")` resolves to the constructor with a single `String` parameter. The JVM selects it by parameter types, not by name.

______________________________________________________________________

**Summary:**
`this(...)` invokes another constructor selected by parameter types. It creates a chain of constructor calls without naming constructors explicitly.


______________________________________________________________________

## Difference Between a Class Constructor and the `public static void main(String[] args)` Method

______________________________________________________________________

### **Quick Reference Table**


| Feature                 | Constructor                        | `public static void main(String[] args)` |
|:------------------------|:-----------------------------------|:-----------------------------------------|
| Purpose                 | Initializes new objects            | Entry point for the application          |
| Who Calls It?           | JVM (when `new` is used)           | JVM (when you run the class)             |
| Name                    | Same as class name                 | Always `main`                            |
| Return Type             | None (not even `void`)             | Must be `void`                           |
| Static?                 | Never static                       | Always static                            |
| Runs Automatically?     | Yes, on object creation            | Yes, when app starts                     |
| Can Have Parameters?    | Yes, (overloading allowed)         | Yes, but always `String[] args`          |
| Used For                | Setting up object state            | Running top-level logic/program startup  |
| Typical Location        | Any class that can be instantiated | Main/launcher/entry-point class          |
| Can Be Overloaded?      | Yes                                | No (signature always the same)           |
| Can Be Called Directly? | No (use `new ClassName(...)`)      | No, JVM calls it automatically           |


______________________________________________________________________

### What Is a Constructor?


- **Purpose:** Automatically initializes new objects, possibly with supplied data.
- **Declaration:** No return type; uses the class name.


```java
public class Car {
    public Car(String make) {
        // Initialization
    }
}
```


- **Triggered By:** Creating objects: `Car car = new Car("Toyota");`


______________________________________________________________________

### What Is `public static void main(String[] args)`?


- **Purpose:** Serves as the *program's entry point*—the starting place when you run your Java app.
- **Declaration:**


```java
public static void main(String[] args) {
    // Startup code here
}
```


- **Triggered By:**
	- Running the class: `java MyApp`
	- The JVM looks for this specific method, no matter what the class is named.


______________________________________________________________________

### Key Differences


- **Constructor:**
	- Runs every time an *object* is created.
	- Used to set up initial values/state for the object.
	- Not static; tied to object instantiation.
- **main() method:**
	- Runs *only once* when the program starts.
	- Used to execute the *program logic or test/demo code*.
	- Is static; not tied to object creation—no object required.[GeeksforGeeks](https://www.geeksforgeeks.org/java/constructor-chaining-java-examples/)[Baeldung](https://www.baeldung.com/java-chain-constructors)[scaler](https://www.scaler.com/topics/constructor-chaining-in-java/)


______________________________________________________________________

> [!TIP] Quick Mental Model
> `main` is the program entry point; a constructor initializes each object.

______________________________________________________________________

**Summary:**
A constructor initializes a new object. `main` is the JVM entry point for starting application logic. They serve different purposes.










#java #oop #constructors #overloading
