
# Data Types and Classes

What are the Integer and Boolean wrapper classes?
When and how do you use the var keyword?
Strings and Arrays
Are strings and arrays mutable or immutable?
How do you use string split and substring?
How can regex be used as a delimiter?
Enums
What are enums and when are they used?
Why are enums similar to final constants?
What is the naming convention for final constants?
Object Creation
How can you create objects without using the new keyword?
Methods
What are method interfaces?
What is method overloading?
What is method overriding?
Can the main method be overloaded?
Why should methods only return values and not contain System.out.println?

Here’s a comprehensive, cheatsheet-style answer to your Java questions, formatted for clarity and quick reference in Obsidian:

***

## Data Types and Classes

### Integer and Boolean Wrapper Classes

- **Integer** wraps the primitive `int`. Use it when you need an integer as an object (e.g., in collections, generics, or APIs that require objects). Contains useful methods like `parseInt()`, `toString()`, and static constants like `Integer.MAX_VALUE`.[^1][^2]
- **Boolean** wraps the primitive `boolean`. Required for Java generics (e.g., `List<Boolean>`) or when you need a boolean as an object. Static instances `Boolean.TRUE`/`FALSE` avoid object creation.[^3]

```java
Integer number = Integer.valueOf(42);   // Wrapping
int unwrapped = number.intValue();      // Unwrapping

Boolean flag = Boolean.valueOf(true);   // Wrapping
boolean b = flag.booleanValue();        // Unwrapping
```


### Using the `var` Keyword

- **Syntax:** `var variableName = initialValue;`
- **Purpose:** Local variable type inference (compiler infers type from initializer).[^4][^5]
- **Usage:** Best for complex or verbose generic types (e.g., `var list = new ArrayList<String>()`), or simple locals when type is obvious.
- **Not usable:** For fields, method parameters, or return types.
- **Java 10+** only.

```java
var message = "Hello, world";        // String
var nums = List.of(1, 2, 3);         // List<Integer>
```


***

## Strings and Arrays

### Mutability

- **Strings** are **immutable**: Once created, their value cannot change. Operations like `concat()` or `replace()` return new `String` objects.[^6]
- **Arrays** are **mutable**: Their elements can be modified after creation. However, the array’s length and type are fixed.[^7][^8]

```java
String s = "Java";
s = s.concat(" is cool");   // s now refers to a new String

int[] arr = {1, 2, 3};
arr[0] = 10;                // Modifies array contents
```


### String `split()` and `substring()`

- **`split(String regex)`**: Splits a string into an array using a regular expression as the delimiter. Returns an array of `String`.[^9][^10]
- **`substring(int beginIndex)`**: Returns a new string from `beginIndex` (inclusive) to the end.
- **`substring(int beginIndex, int endIndex)`**: Returns a new string from `beginIndex` (inclusive) to `endIndex` (exclusive).[^11][^12]

```java
String csv = "one,two,three";
String[] parts = csv.split(",");           // ["one", "two", "three"]
String middle = csv.substring(4, 7);       // "two"
String end = csv.substring(5);             // "wo,three"
```


### Regex as Delimiter

Use **regular expressions** in `split()` for complex delimiters. For multiple delimiters, specify them inside `[]`, e.g., `split("[ ,.-]")` splits on comma, space, period, or dash.[^13][^10]

***

## Enums

### What Are Enums?

- **Enums** define a fixed set of named constants, e.g., days of the week or status codes. They are type-safe and can have methods and fields.
- **Usage:** When you have a predefined list of values that are meaningful and unlikely to change.

```java
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }
```


### Enums vs. Final Constants

- **Enums** are typesafe, support methods, and can be iterated.
- **Final constants** (`static final int MONDAY = 0;`) are just values—no type safety, no iteration.
- **Enums are superior** for representing a fixed set of related values.


### Naming Convention for Final Constants

- **Constants:** Use `ALL_CAPS` with underscores (e.g., `MAX_VALUE`).
- **PascalCase** is for class, enum, and interface names.

***

## Object Creation

### Create Objects Without `new`

- **Factory Methods:** Use static factory methods provided by some classes (e.g., `Integer.valueOf(5)`, `Boolean.valueOf(true)`).
- **Reflection:** Advanced—use `Class.forName(...).newInstance()`, but avoid unless necessary.
- **Deserialization:** Read objects from files/streams.
- **Clone:** Use `clone()` method (must implement `Cloneable`).
- **No `new` for primitives:** Primitive types (e.g., `int`, `boolean`) are not objects and do not use `new`.

***

## Methods

### Method Interfaces

**Method interfaces** (functional interfaces) are interfaces with a single abstract method. Example: `Predicate<T>`, `Consumer<T>`. Used in lambdas and method references.

### Method Overloading

- **Overloading:** Multiple methods with the same name but different parameter lists (types or counts).
- **Used** for convenience when similar operations are performed on different parameter sets.

```java
void print(int x) { ... }
void print(String s) { ... }
void print(int x, String s) { ... }
```


### Method Overriding

- **Overriding:** Subclass redefines a method already defined in its superclass.
- **Used** to change behavior in subclasses (polymorphism).


### Can `main` Method Be Overloaded?

Yes, you can **overload** the `main` method—only `public static void main(String[] args)` is called automatically by the JVM. Other versions must be called explicitly.

### Why Should Methods Only Return Values?

- **Separation of concerns:** Methods should perform a single task.
- **Reusability:** Methods that return values can be used in a variety of contexts (e.g., logged, displayed, or further processed).
- **Testability:** Methods that only return values are easier to unit test.
- **Avoid `System.out.println` in business logic:** Output should be handled by the caller, not the function itself.

***

## Quick Reference Table

| Concept              | Key Details                                               |
|:---------------------|:----------------------------------------------------------|
| Integer/Boolean      | Object wrappers for primitive types, used in generics     |
| `var`                | Local type inference (Java 10+), not for fields/methods   |
| String mutability    | Immutable; operations return new String objects           |
| Array mutability     | Mutable (elements can change; length/type fixed)          |
| `split`/`substring`  | Split by regex; extract substrings with indexes           |
| Regex delimiter      | Use regex in `split` for complex splitting                |
| Enums                | Fixed, typesafe constants with methods                    |
| Final constants      | Use `ALL_CAPS` for static final fields                    |
| Object without `new` | Factory methods, reflection, deserialization, clone       |
| Method overloading   | Same name, different parameters                           |
| Method overriding    | Subclass redefines superclass method for polymorphism     |
| `main` overloading   | Allowed, but only `main(String[])` is auto-called         |
| Method return values | Prefer return values over direct output—better separation |

***

## Callouts

> [!TIP]
> Use factory methods (`valueOf`) for common objects instead of constructors—they may reuse instances.

> [!WARNING]
> String immutability means every modification creates a new object. For heavy string manipulation, consider `StringBuilder` or `StringBuffer`.

> [!EXAMPLE]
> Enum with methods:
> ```java
> enum Planet {
>   EARTH(6371), MARS(3390);
>   private final int radius;
>   Planet(int r) { radius = r; }
>   public int getRadius() { return radius; }
> }
> ```

***

# Tags

\#java \#wrapper-classes \#enums \#methods \#strings \#arrays \#bestpractices \#cheatsheet


[^1]: https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html

[^2]: https://www.geeksforgeeks.org/java/java-lang-integer-class-java/

[^3]: https://www.geeksforgeeks.org/java/java-lang-boolean-class-java/

[^4]: https://www.geeksforgeeks.org/java/var-keyword-in-java/

[^5]: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/var-in-java-keyword-type-inferred-benefits-examples-final-scope-rules

[^6]: https://www.baeldung.com/java-mutable-string

[^7]: https://sebhastian.com/are-arrays-mutable-java/

[^8]: https://stackoverflow.com/questions/16125616/are-string-arrays-mutable

[^9]: https://www.w3schools.com/java/ref_string_split.asp

[^10]: https://www.geeksforgeeks.org/java/split-string-java-examples/

[^11]: https://beginnersbook.com/2013/12/java-string-substring-method-example/

[^12]: https://www.digitalocean.com/community/tutorials/java-string-substring

[^13]: https://beginnersbook.com/2022/09/split-string-by-multiple-delimiters-in-java/

[^14]: https://www.w3schools.com/java/java_wrapper_classes.asp

[^15]: https://runestone.academy/ns/books/published/csawesome/Unit2-Using-Objects/topic-2-8-IntegerDouble.html

[^16]: https://www.codekru.com/java/integer-wrapper-class-in-java

[^17]: https://selenium-by-arun.blogspot.com/2014/08/242-integer-wrapper-class.html

[^18]: https://stackoverflow.com/questions/4890802/how-does-the-java-boolean-wrapper-class-get-instantiated

[^19]: https://briebug.com/java-split-string/

