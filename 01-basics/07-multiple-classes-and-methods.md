---
tags: [java/oop, classes, methods, this-keyword, basics]
date: 2025-08-23
topic: Introduction to Classes and Objects in Java
---

______________________________________________________________________

# 🏎️ Introduction to Classes and Objects in Java

tags: #java #oop #classes #objects #fundamentals
date: 2025-08-22

______________________________________________________________________

<!-- TOC -->
* [🏎️ Introduction to Classes and Objects in Java](#-introduction-to-classes-and-objects-in-java)
  * [🔑 Core Concepts](#-core-concepts)
  * [📝 Example: Car.java](#-example-carjava)
    * [Step 1: Defining a Class with Attributes](#step-1-defining-a-class-with-attributes)
    * [Explain Code](#explain-code)
    * [Step 2: Adding a Constructor](#step-2-adding-a-constructor)
    * [Explain Code](#explain-code-1)
    * [Step 3: Adding a Method](#step-3-adding-a-method)
    * [Explain Code](#explain-code-2)
  * [📝 Example: CarDemo.java](#-example-cardemojava)
    * [Step 1: Setting Up a Driver Class](#step-1-setting-up-a-driver-class)
    * [Explain Code](#explain-code-3)
    * [Step 2: Creating and Using Objects](#step-2-creating-and-using-objects)
    * [Explain Code](#explain-code-4)
  * [🛠️ Compiling and Running Multiple Classes](#-compiling-and-running-multiple-classes)
  * [✅ Expected Output](#-expected-output)
  * [🚀 Summary](#-summary)
  * [**Understanding the `this` Keyword in Java**](#understanding-the-this-keyword-in-java)
    * [**Quick Reference**](#quick-reference)
    * [1️⃣ Referring to Instance Variables (Shadowed by Parameters)](#1-referring-to-instance-variables-shadowed-by-parameters)
    * [2️⃣ Invoking Other Constructors in the Same Class](#2-invoking-other-constructors-in-the-same-class)
    * [3️⃣ Returning the Current Object](#3-returning-the-current-object)
    * [4️⃣ Passing the Current Object as a Parameter](#4-passing-the-current-object-as-a-parameter)
    * [5️⃣ Invoking Another Method on the Same Object](#5-invoking-another-method-on-the-same-object)
    * [6️⃣ `this` in Getters and Setters](#6-this-in-getters-and-setters)
    * [🌟 Summary Table](#-summary-table)
    * [👉 Practical Tips](#-practical-tips)
  * [Why `public static void main(String[] args)` Is in the Demo Class (Not the Car Class)](#why-public-static-void-mainstring-args-is-in-the-demo-class-not-the-car-class)
    * [**Quick Reference**](#quick-reference-1)
    * [💡 Why Main Is in `CarDemo` Instead of `Car`](#-why-main-is-in-cardemo-instead-of-car)
      * [**Design Principle: Separate Logic from Data**](#design-principle-separate-logic-from-data)
    * [🕹️ Role of Each Class](#-role-of-each-class)
    * [📢 What Happens If You Put `main` in the Model Class?](#-what-happens-if-you-put-main-in-the-model-class)
    * [🚀 How the JVM Launches Your Program](#-how-the-jvm-launches-your-program)
    * [📝 Main Method Recap](#-main-method-recap)
    * [🗂️ Related Reading in Your Vault](#-related-reading-in-your-vault)
<!-- TOC -->

## 🔑 Core Concepts


- **Class**: A blueprint/template for creating objects.
- **Object**: An instance of a class (real-world entity).
- **Encapsulation**: Protecting data inside a class using access modifiers like `private`.
- **Constructor**: A special method to initialize new objects.
- **Methods**: Define behaviors (actions) objects can perform.


> [!TIP]
> Think of a **class as the cookie cutter** (blueprint) and **objects as the cookies** made with it.

______________________________________________________________________

## 📝 Example: Car.java

### Step 1: Defining a Class with Attributes


```java
public class Car {
    // Attributes (also called fields/properties)
    private String make;
    private String model;
    private int year;
}
```


### Explain Code


- `public class Car { ... }` → Declares a public class named `Car`.
- Attributes represent the **state of each object**:
	- `make` → Manufacturer name (e.g., Toyota)
	- `model` → Car model (e.g., Corolla)
	- `year` → Year of manufacture (e.g., 2022)
- `private` keyword → Attributes are hidden from outside access (**Encapsulation**).


______________________________________________________________________

### Step 2: Adding a Constructor


```java
// Constructor: runs automatically when a new Car object is created
public Car(String make, String model, int year) {
    this.make = make;   // 'this.make' refers to the class attribute
    this.model = model;
    this.year = year;
}
```


### Explain Code


- A **constructor** has the **same name as the class** (`Car`) and **no return type**.
- Parameters (`String make, String model, int year`) provide initial values.
- `this` keyword → Refers to the current object.
	- Helps distinguish between class attributes (`this.make`) and constructor parameters (`make`).


______________________________________________________________________

### Step 3: Adding a Method


```java
// Method that displays car details
public void displayInfo() {
    System.out.println("Car Information:");
    System.out.println("Make: " + make);
    System.out.println("Model: " + model);
    System.out.println("Year: " + year);
}
```


### Explain Code


- **Methods** define what an object can do (behavior).
- `void` means it doesn’t return anything.
- `displayInfo()` prints car details.


> [!EXAMPLE]
> When you call `myCar.displayInfo()`, it prints that particular car’s details.

______________________________________________________________________

## 📝 Example: CarDemo.java

### Step 1: Setting Up a Driver Class


```java
public class CarDemo {
    public static void main(String[] args) {
        // We'll add code here in the next step
    }
}
```


### Explain Code


- `CarDemo` → A separate class with a `main` method (entry point).
- `main(String[] args)` → Where the program starts running.


______________________________________________________________________

### Step 2: Creating and Using Objects


```java
public class CarDemo {
    public static void main(String[] args) {
        // Create first Car object
        Car myCar = new Car("Toyota", "Corolla", 2022);
        myCar.displayInfo();

        // Create second Car object
        Car friendsCar = new Car("Honda", "Civic", 2023);
        friendsCar.displayInfo();
    }
}
```


### Explain Code


- `new Car("Toyota", "Corolla", 2022)` → Creates a new `Car` object using the constructor.
- Objects:
	- `myCar` → represents your Toyota Corolla (2022).
	- `friendsCar` → represents a Honda Civic (2023).
- Calls `displayInfo()` → Prints car information.


______________________________________________________________________

## 🛠️ Compiling and Running Multiple Classes

If **both files are in the same directory**:


```bash
javac Car.java CarDemo.java   # Compile both files
java CarDemo                  # Run the driver class
```


If **Car is in a package**, import is needed:


```java
import packagename.Car;
```


If in different directories:


```bash
java -cp ~/project CarDemo
```


______________________________________________________________________

## ✅ Expected Output


```
Car Information:
Make: Toyota
Model: Corolla
Year: 2022
Car Information:
Make: Honda
Model: Civic
Year: 2023
```


______________________________________________________________________

## 🚀 Summary


- **Class = blueprint**, **Object = instance**
- **Attributes** store data (encapsulation keeps them private)
- **Constructor** initializes objects
- **Methods** define behavior
- `new` keyword creates objects in memory
- Keep `Car.java` and `CarDemo.java` in the same folder for simple compilation


______________________________________________________________________

> [!INFO] Real-world use
> This pattern (Class + Demo class) is common for OOP exercises. In larger projects, you’ll use **packages** and *
*import
> statements** to organize classes.


______________________________________________________________________

## **Understanding the `this` Keyword in Java**

tags: #java #oop #this #bestpractices
date: 2025-08-22

______________________________________________________________________

### **Quick Reference**


- **`this`** refers to the **current object** (the object whose method or constructor is being executed)
- Common uses:
	- Distinguish between fields and parameters with the same name
	- Call another constructor in the same class
	- Return the current object
	- Pass the current object to another method or constructor
	- Invoke another method on the current object


______________________________________________________________________

### 1️⃣ Referring to Instance Variables (Shadowed by Parameters)

When a method or constructor parameter has the same name as a class field, `this` disambiguates between them:


```java
public class Car {
    private String make;

    public Car(String make) {
        this.make = make; // this.make is the field, make is the parameter
    }
}
```


> [!TIP]
> Without `this`, `make = make;` would set the parameter to itself, leaving the field unchanged!

______________________________________________________________________

### 2️⃣ Invoking Other Constructors in the Same Class

Use `this()` to call a different constructor in the same class (constructor chaining):


```java
public class Car {
    private String make;
    private String model;

    public Car() {
        this("Unknown", "Unknown"); // Calls two-arg constructor
    }

    public Car(String make, String model) {
        this.make = make;
        this.model = model;
    }
}
```


> [!NOTE]
> `this()` must be the first statement in a constructor.[^5][^1]

______________________________________________________________________

### 3️⃣ Returning the Current Object

Useful in method chaining patterns (e.g., builder pattern):


```java
public class Car {
    private String make;

    public Car setMake(String make) {
        this.make = make;
        return this;
    }
}

// Usage:
Car c = new Car().setMake("Toyota");
```


> [!EXAMPLE]
> Enables chaining: `c.setMake(...).setModel(...).setYear(...);`

______________________________________________________________________

### 4️⃣ Passing the Current Object as a Parameter

You can send `this` as an argument to another method or constructor:


```java
public class Car {
    public void printCar(Car car) { /* ... */ }

    public void show() {
        printCar(this);
    }
}
```


> [!USE CASE]
> Helpful for callback patterns or registration methods.[^1][^5]

______________________________________________________________________

### 5️⃣ Invoking Another Method on the Same Object

Explicitly calling a method of the same object (optional, but clarifies intent):


```java
public void displayInfo() {
    this.printHeader(); // Calls current object's method
    // ...
}
```


- Not strictly required; using just `printHeader()` works the same.[^7][^5]


______________________________________________________________________

### 6️⃣ `this` in Getters and Setters

Common in setter methods to avoid shadowing:


```java
public void setModel(String model) {
    this.model = model;
}

public String getModel() {
    return this.model; // 'this' optional, but often used for clarity
}
```


> [!TIP]
> Use `this` in setters, especially if parameter and field names overlap.[^2][^6][^5]

______________________________________________________________________

### 🌟 Summary Table


| Use Case                   | Example Syntax        | Typical Context                   |
|:---------------------------|:----------------------|:----------------------------------|
| Distinguish field/param    | `this.x = x;`         | Constructors & Setters            |
| Call another constructor   | `this(...);`          | Constructor chaining              |
| Return current object      | `return this;`        | Method chaining/fluent APIs       |
| Pass to method/constructor | `someMethod(this);`   | Callbacks, listeners, composition |
| Invoke current method      | `this.doSomething();` | Clarify call within self          |


______________________________________________________________________

### 👉 Practical Tips


- Use `this` whenever **local/parameter names shadow fields**.
- For method chaining, always `return this;` for setter-style methods.
- In complex classes, explicit `this` improves readability and maintainability.
- `this` cannot be used in static methods (no current object exists).


______________________________________________________________________

## Why `public static void main(String[] args)` Is in the Demo Class (Not the Car Class)

tags: #java #mainmethod #structure #bestpractices
date: 2025-08-22

______________________________________________________________________

### **Quick Reference**


- **`public static void main(String[] args)`** is the **entry point** of any standalone Java application.
- The **JVM always looks for this method to start your program**.
- It's placed in the class responsible for launching/running the application logic – in this example: `CarDemo`.


______________________________________________________________________

### 💡 Why Main Is in `CarDemo` Instead of `Car`

#### **Design Principle: Separate Logic from Data**


- `Car` represents a **blueprint/data model** for car objects.
	- It holds data (`make`, `model`, `year`).
	- Defines how a `Car` behaves (methods like `displayInfo()`).
- `CarDemo` represents **the code that “drives” or tests the cars**.
	- Contains the main routine—what happens first.
	- Creates `Car` objects and calls their methods.


> [!TIP]
> Putting `main` in a separate class (such as `CarDemo`, `Main`, or `App`) keeps your core **object logic clean and
> reusable**. It separates **application startup code** from **data structures**.[^1][^5]

______________________________________________________________________

### 🕹️ Role of Each Class


| Class     | Purpose                          | Contains `main()`? |
|:----------|:---------------------------------|:-------------------|
| `Car`     | Blueprint/model for car objects  | ❌ (NO)             |
| `CarDemo` | Program entry point, test driver | ✅ (YES)            |


______________________________________________________________________

### 📢 What Happens If You Put `main` in the Model Class?


- You **can** put `main` into `Car`, but then `Car.java` is both a data model and an entry point.
- This **mixes responsibilities** and is not recommended for real-world code.
- For clarity, scalability, and good design, place `main` in a **dedicated launcher/demo class**.[^3][^5][^1]


______________________________________________________________________

### 🚀 How the JVM Launches Your Program


1. You run:


```
java CarDemo
```


2. The JVM **loads the `CarDemo` class** and looks for:


```java
public static void main(String[] args)
```


3. The code in that method runs, creating and using `Car` objects.


______________________________________________________________________

### 📝 Main Method Recap


```java
public static void main(String[] args)
```


- `public` – JVM must access this method from outside the class.
- `static` – No need to create an object to run this; JVM can call it directly.
- `void` – Returns nothing.
- `main` – Special method name that’s a convention the JVM requires.
- `String[] args` – Receives command-line arguments.


> [!INFO]
> Every Java application must have a class with a `public static void main(String[] args)` method as its entry
> point.[^2][^5][^1]

______________________________________________________________________

### 🗂️ Related Reading in Your Vault


- \[[Project Structure Best Practices]\]
- \[[Separation of Concerns]\]
- \[[Java Application Entry Points]\]


______________________________________________________________________

**Summary:**
Keep `public static void main(String[] args)` in a dedicated, top-level class like `CarDemo` to keep your model classes
focused and your project organized and maintainable.


[^1]: https://www.digitalocean.com/community/tutorials/public-static-void-main-string-args-java-main-method

[^3]: https://stackoverflow.com/questions/24097806/where-should-i-put-the-public-static-void-mainstring-args-method

[^5]: https://www.tutorialspoint.com/java-public-static-void-main-string-args

[^6]: https://www.geeksforgeeks.org/java/understanding-static-in-public-static-void-main-in-java/

[^7]: https://www.youtube.com/watch?v=P-_Nzi_mCRo

[^2]: https://www.geeksforgeeks.org/java/java-main-method-public-static-void-main-string-args/

[^4]: https://www.reddit.com/r/learnprogramming/comments/5v810b/what_is_public_static_void_mainstring_args/

[^8]: https://study.com/academy/lesson/what-is-public-static-void-main-in-java.html

#java #basics #classes #methods
