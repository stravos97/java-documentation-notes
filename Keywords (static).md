# java keywords e.g. static etc

yaml
***
tags: [java, reference, keywords, cheatsheet]
date: 2025-08-22
topic: Java Keywords Overview
***

## Java Keywords: Quick Reference

\#java \#keywords \#syntax \#basics [[Java Syntax]]

> [!NOTE]
> ==Java keywords== are reserved words with special meaning in the Java language. They **cannot be used as identifiers** (like variable or method names).[^3][^5]
> As of Java 21, there are 53 keywords, plus some reserved but unused and "literal" values.

***

## Table: Common Java Keywords and Their Use

| Keyword      | Usage/purpose                                                               |
| :----------- | :-------------------------------------------------------------------------- |
| abstract     | Declare abstract classes or methods (no implementation, must be overridden) |
| assert       | Test assumptions in code                                                    |
| boolean      | Boolean data type (true/false)                                              |
| break        | Exit a loop or switch                                                       |
| byte         | 8-bit integer data type                                                     |
| case         | Define branch in switch statement                                           |
| catch        | Handle exceptions after try block                                           |
| char         | 16-bit Unicode character                                                    |
| class        | Declare a class                                                             |
| const        | Reserved (not used)                                                         |
| continue     | Skip to next iteration of loop                                              |
| default      | Default branch in switch; default method in interface                       |
| do           | Start of do-while loop                                                      |
| double       | 64-bit floating point number                                                |
| else         | Branch for if statement                                                     |
| enum         | Create enumerations                                                         |
| extends      | Inherit class or interface                                                  |
| final        | Prevent override or change (variable, method, class)                        |
| finally      | Always executes after try/catch                                             |
| float        | 32-bit floating point number                                                |
| for          | Loop structure                                                              |
| goto         | Reserved (not used)                                                         |
| if           | Conditional branch                                                          |
| implements   | Declare that a class implements interfaces                                  |
| import       | Include other Java classes/packages                                         |
| instanceof   | Test object type                                                            |
| int          | 32-bit integer data type                                                    |
| interface    | Declare an interface                                                        |
| long         | 64-bit integer data type                                                    |
| native       | Method written in platform-native code                                      |
| new          | Create new objects                                                          |
| null         | Reference to no object                                                      |
| package      | Specify namespace                                                           |
| private      | Accessible only within class                                                |
| protected    | Accessible within package and subclasses                                    |
| public       | Accessible from anywhere                                                    |
| return       | Return from a method                                                        |
| short        | 16-bit integer data type                                                    |
| static       | Belongs to class rather than instance                                       |
| strictfp     | Restrict floating-point arithmetic for portability                          |
| super        | Access parent class members                                                 |
| switch       | Multi-branch selection                                                      |
| synchronized | Thread-safe code blocks                                                     |
| this         | Reference current object                                                    |
| throw        | Raise an exception                                                          |
| throws       | Declare exception(s) a method may throw                                     |
| transient    | Not serializable                                                            |
| try          | Start exception-handling block                                              |
| void         | No return value                                                             |
| volatile     | May be modified by multiple threads                                         |
| while        | Begin while loop                                                            |
| sealed       | Restricts which other classes may extend or implement this class/interface  |
| permits      | Specifies which classes may extend a sealed class                           |


***

## Special Notes

- **true, false, null:** Not keywords, but ==literals== (still can't use as identifiers).[^5][^3]
- **const, goto:** Reserved, but not used.[^3][^5]
- **case-sensitive:** All keywords must be ==lowercase==.
- **static:** Denotes static variables/methods (shared across all instances).

> [!TIP]
> Use keywords for their intended purpose only. Using them otherwise results in compiler errors.

***

## Example Usage

```java
public class Example {
    public static void main(String[] args) {
        final int MAX = 5;
        for (int i = 0; i < MAX; i++) {
            if (i % 2 == 0) continue;
            System.out.println(i);
        }
    }
}
```

> Demonstrates use of: `public`, `class`, `final`, `int`, `for`, `if`, `continue`, `System`

***

\#java/reference \#syntax \#cheatsheets
<span style="display:none">[^1][^2][^4][^6][^7]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.w3schools.com/java/java_ref_keywords.asp

[^2]: https://en.wikipedia.org/wiki/List_of_Java_keywords

[^3]: https://www.geeksforgeeks.org/java/java-keywords/

[^4]: https://data-flair.training/blogs/java-keywords/

[^5]: https://www.codejava.net/java-core/the-java-language/java-keywords

[^6]: https://www.programiz.com/java-programming/keywords-identifiers

[^7]: http://key.mkprog.com/en/java/


***
tags: [java, keywords, beginner, reference, cheatsheet]
date: 2025-08-22
topic: Java Keywords - Beginner-Friendly Guide
***

# Java Keywords: Complete Beginner's Guide

\#java \#keywords \#basics \#beginners [[Java Syntax]]

> [!NOTE]
> **Java keywords** are ==reserved words== with special meanings that you **cannot** use as variable names, method names, or class names. Java currently has about **67 keywords** (the number changes with new versions).

***

## Why Keywords Matter

Think of keywords as **the building blocks** of Java - they tell the computer exactly what you want to do. Every Java program uses keywords to:

- Create classes (`class`)
- Define methods (`public`, `static`, `void`)
- Control program flow (`if`, `else`, `for`)
- Manage data (`int`, `String`, `boolean`)

***

## Keywords by Category

### 🏗️ **Class \& Object Keywords**

#### `class`

**What it does:** Creates a new class (like a blueprint for objects)

```java
class MyClass {
    // Your code here
}
```


#### `public`

**What it does:** Makes something accessible from anywhere

```java
public class MyClass {        // Anyone can use this class
    public void sayHello() {  // Anyone can call this method
        System.out.println("Hello!");
    }
}
```


#### `private`

**What it does:** Hides something - only the same class can access it

```java
class BankAccount {
    private double balance;  // Only this class can access balance
    
    public double getBalance() {  // Public method to safely access balance
        return balance;
    }
}
```

> [!TIP]
> **Rule of thumb:** Use `private` for data you want to protect, `public` for things others need to use.

#### `protected`

**What it does:** Accessible within the same package OR by subclasses (child classes)

```java
class Animal {
    protected String name;  // Child classes can access this
}

class Dog extends Animal {
    public void bark() {
        System.out.println(name + " says woof!");  // Can use 'name' here
    }
}
```


***

###  **Static Keyword**

#### `static`

**What it does:** Makes something belong to the **class itself**, not individual objects.[^2][^3][^4]

**Key Points:**

- You can use static things **without creating objects**
- All objects **share** the same static variable
- Static methods **cannot** access non-static variables

```java
class Counter {
    static int count = 0;  // Shared by ALL Counter objects
    
    static void increment() {  // Can call without making object
        count++;
    }
}

// Usage:
Counter.increment();  // No need to create object!
System.out.println(Counter.count);  // Prints: 1
```

**Real Example:** The `main` method is always static!

```java
public static void main(String[] args) {
    // Static so Java can run it without creating objects
}
```

> [!EXAMPLE]
> **Math class:** `Math.abs(-5)`, `Math.max(3, 7)` - all static methods you can use directly!

***

### 🔄 **Control Flow Keywords**

#### `if` and `else`

**What they do:** Make decisions in your code

```java
int age = 18;
if (age >= 18) {
    System.out.println("You can vote!");
} else {
    System.out.println("Too young to vote");
}
```


#### `for`

**What it does:** Repeats code a specific number of times

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Count: " + i);
}
// Prints: Count: 0, Count: 1, Count: 2, Count: 3, Count: 4
```


#### `while`

**What it does:** Repeats code while a condition is true

```java
int count = 0;
while (count < 3) {
    System.out.println("Hello!");
    count++;
}
```


#### `break` and `continue`

**What they do:** Control loops

```java
for (int i = 0; i < 10; i++) {
    if (i == 3) continue;  // Skip when i is 3
    if (i == 7) break;     // Stop the loop when i is 7
    System.out.println(i);
}
// Prints: 0, 1, 2, 4, 5, 6
```


***

### 📊 **Data Type Keywords**

#### Primitive Types

```java
int age = 25;           // Whole numbers
double price = 19.99;   // Decimal numbers
boolean isReady = true; // true or false only
char grade = 'A';       // Single character
```

> [!NOTE]
> These are **not objects** - they store actual values, not references.

***

### 🔧 **Method \& Variable Keywords**

#### `void`

**What it does:** Method returns nothing

```java
void printMessage() {  // No return value
    System.out.println("Hello!");
}
```


#### `return`

**What it does:** Sends a value back from a method

```java
int addNumbers(int a, int b) {
    return a + b;  // Sends back the sum
}
```


#### `final`

**What it does:** Makes something unchangeable

```java
final int MAX_STUDENTS = 30;  // Cannot change this value
MAX_STUDENTS = 40;  // ERROR! Cannot modify final variable
```


***

### 🏛️ **Object-Oriented Keywords**

#### `extends`

**What it does:** Creates a child class (inheritance)

```java
class Animal {
    void eat() { System.out.println("Eating..."); }
}

class Dog extends Animal {  // Dog inherits from Animal
    void bark() { System.out.println("Woof!"); }
}
```


#### `this`

**What it does:** Refers to the current object

```java
class Person {
    String name;
    
    void setName(String name) {
        this.name = name;  // 'this.name' = object's name, 'name' = parameter
    }
}
```


#### `super`

**What it does:** Refers to the parent class

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    void makeSound() {
        super.makeSound();  // Call parent's method first
        System.out.println("Woof!");
    }
}
```


***

## Common Beginner Mistakes

> [!WARNING]
> **Don't do this:**
> ```java > int class = 5;  // ERROR: 'class' is a keyword! > String public;  // ERROR: 'public' is a keyword! > ```

> [!WARNING]
> **Keywords are case-sensitive:**
> ```java > Public class Test { }  // ERROR: Should be 'public' (lowercase) > ```

> [!WARNING]
> **Static methods can't access non-static variables:**
> ```java > class MyClass { >     int number = 5; >      >     static void printNumber() { >         System.out.println(number);  // ERROR: Can't access non-static from static >     } > } > ```

***

## Quick Reference Table

| Keyword | Purpose | Example |
| :-- | :-- | :-- |
| `public` | Accessible everywhere | `public void method()` |
| `private` | Only same class can access | `private int value;` |
| `static` | Belongs to class, not object | `static int count;` |
| `final` | Cannot be changed | `final int MAX = 100;` |
| `if/else` | Make decisions | `if (x > 5) { ... }` |
| `for` | Loop specific times | `for (int i = 0; i < 10; i++)` |
| `void` | Method returns nothing | `void printHello()` |
| `return` | Return value from method | `return x + y;` |
| `this` | Current object | `this.name = name;` |
| `extends` | Inherit from parent class | `class Dog extends Animal` |


***

## Memory Tips

> [!TIP]
> **Remember static:** **S**hared across all objects, **S**tatic methods don't need objects to run
>
> **Remember access modifiers:**
> - `public` = **P**ublic restroom (everyone can use)
> - `private` = **P**rivate bedroom (only you)
> - `protected` = **P**rotected family room (family + close friends)

***

\#java/basics \#java/keywords \#programming \#cheatsheets
<span style="display:none">[^10][^11][^12][^13][^14][^5][^6][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.youtube.com/watch?v=tfLZmZsRRxc

[^2]: https://www.geeksforgeeks.org/java/static-keyword-java/

[^3]: https://www.w3schools.com/java/ref_keyword_static.asp

[^4]: https://www.freecodecamp.org/news/java-static-keyword-explained/

[^5]: https://www.youtube.com/watch?v=Zw4mAxm5pCA

[^6]: https://www.baeldung.com/java-access-modifiers

[^7]: https://www.geeksforgeeks.org/java/java-keywords/

[^8]: https://www.geekster.in/articles/static-keyword-in-java/

[^9]: https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers

[^10]: https://www.geeksforgeeks.org/java/public-vs-protected-vs-package-vs-private-access-modifier-in-java/

[^11]: https://beginnersbook.com/2022/10/java-keywords-with-examples/

[^12]: https://www.simplilearn.com/tutorials/java-tutorial/static-keyword-in-java

[^13]: https://www.w3schools.com/java/java_modifiers.asp

[^14]: https://www.youtube.com/watch?v=T632kAJ_9VA

