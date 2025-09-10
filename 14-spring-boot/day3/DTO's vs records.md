# DTO vs Record in Java: Which Should You Use?

## Understanding the Differences and Use Cases for Data Transfer Objects (DTOs) and Java Records in Modern Java Development

**Author**: Ashutosh Krishna  
**Date**: Sep 29, 2024  
**Reading Time**: 13 min read

---
---

In Java applications, we often need to transfer data between different layers of the application, or between services. For this purpose, we use Data Transfer Objects (DTOs). A DTO is a simple object designed to hold data, without any complex behavior or logic. Its job is to bundle data and pass it along where needed.

Now, Java introduced a new feature in Java 14, called Records. These are special types of classes that focus on holding data, just like DTOs. The big difference is that Records do a lot of the repetitive work for us. For example, they automatically create methods to get the data (like getters), and they handle equality checks, `toString()`, and more. This feature became fully available in Java 16, making Records a modern, clean way to work with data in Java.

So, why are we comparing DTOs and Records? Because they both serve a similar purpose — carrying data. However, understanding when to use one over the other is important as Java continues to evolve. In this article, we'll explore the differences and help you decide which one fits your needs better, especially if you're working on modern Java applications.

## What is a DTO?

A Data Transfer Object (DTO) is a simple Java object that is used to move data between different parts of an application. Think of it as a container for carrying data between layers of your application. For example, in a web application, a DTO might be used to transfer data from the service layer to the controller, or from the controller to the view layer.

DTOs help keep the different parts of an application separated, making the code more organized and easier to maintain. They typically don't have any business logic or complex behavior. Instead, they just hold data.

## How are DTOs implemented?

DTOs are usually implemented as regular Java classes. A typical DTO includes:

- Private fields for the data it holds
- Getters and Setters to access and modify the data
- A Constructor to create the object
- Override methods like `toString()`, `hashCode()`, and `equals()` for comparing and printing the object in a meaningful way

Here's an example of a `UserDTO` class:

```java
import java.util.Objects;

public class UserDTO {
    private String name;
    private int age;
    private String email;

    // Constructor
    public UserDTO(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    // Overriding toString method for meaningful output
    @Override
    public String toString() {
        return "UserDTO{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                '}';
    }

    // Overriding equals method for object comparison
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        UserDTO userDTO = (UserDTO) o;
        return age == userDTO.age && Objects.equals(name, userDTO.name) && Objects.equals(email, userDTO.email);
    }

    // Overriding hashCode method
    @Override
    public int hashCode() {
        return Objects.hash(name, age, email);
    }
}
```

This `UserDTO` class holds information about a user: their name, age, and email. It also provides basic functionality like comparing two `UserDTO` objects (using `equals()`), generating a unique hash code (using `hashCode()`), and a `toString()` method for readable output.

With tools like Lombok, you can avoid manually writing boilerplate code while still having a fully functional DTO. However, with Records, as we'll explore later, Java offers an alternative that also eliminates much of the boilerplate but with a different design philosophy (immutability by default).

Here's how you can use the `UserDTO`:

```java
public class UserDTOUsageExample {
    public static void main(String[] args) {
        UserDTO user = new UserDTO("Ashutosh", 25, "ashutosh@example.com");

        // Access data
        System.out.println(user.getName());
        System.out.println(user.getAge());
        System.out.println(user.getEmail());

        // Using the toString() method
        System.out.println(user);

        // Comparing records
        UserDTO anotherUser = new UserDTO("Vishakha", 22, "vishakha@example.com");
        System.out.println(user.equals(anotherUser));
    }
}
```

**Output:**
```
Ashutosh
25
ashutosh@example.com
UserDTO{name='Ashutosh', age=25, email='ashutosh@example.com'}
false
```

## What is a Java Record?

Java Records are a special type of class introduced in Java 14 (as a preview feature) and fully released in Java 16. They simplify the creation of immutable data carriers. Records are designed to hold data in a concise and readable way, and they eliminate much of the boilerplate code that traditional classes, like DTOs, require.

In a traditional DTO, you manually write constructors, getters, `equals()`, `hashCode()`, and `toString()` methods (as we did earlier). With Records, Java generates all of these for you automatically. This makes them ideal for simple, immutable objects whose main job is to carry data.

## Key Features of Java Records

- **Immutable by default**: Once you create a record, you can't change its data (unlike DTOs, which are typically mutable)
- **Compact syntax**: You declare the fields and Java generates the constructor, getters, `equals()`, `hashCode()`, and `toString()` automatically
- **No setters**: Since records are immutable, they don't provide setters

Let's create a `UserRecord` that represents the same user data as our earlier `UserDTO`, but using a record:

```java
public record UserRecord(String name, int age, String email) {
}
```

That's it! With just one line, Java generates:
- A constructor: `new UserRecord(String name, int age, String email)`
- Getters for each field: `name()`, `age()`, and `email()`
- An `equals()` method for comparing two `UserRecord` objects
- A `hashCode()` method to generate a unique hash code
- A `toString()` method that returns a string representation like: `UserRecord[name=Ashutosh, age=25, email=ashutosh@example.com]`

Here's how you can use the `UserRecord`:

```java
public class UserRecordUsageExample {
    public static void main(String[] args) {
        UserRecord user = new UserRecord("Ashutosh", 25, "ashutosh@example.com");

        // Access data
        System.out.println(user.name());
        System.out.println(user.age());
        System.out.println(user.email());

        // Using the toString() method
        System.out.println(user);

        // Comparing records
        UserRecord anotherUser = new UserRecord("Vishakha", 22, "vishakha@example.com");
        System.out.println(user.equals(anotherUser));
    }
}
```

**Output:**
```
Ashutosh
25
ashutosh@example.com
UserRecord[name=Ashutosh, age=25, email=ashutosh@example.com]
false
```

With `UserRecord`, we avoided writing getters, constructors, `equals()`, `hashCode()`, and `toString()` manually. Java Records offer a clean, concise way to create immutable objects that only carry data.

## Why Use Records?

- **Less Boilerplate**: You don't have to write repetitive code like getters or `equals()` methods
- **Immutable by Design**: Ensures the data can't be changed after the object is created, making it safer to use in multi-threaded environments
- **Clear Intent**: Using a Record clearly communicates that the object is just for carrying data, without additional behavior or logic

## Comparing DTO and Record

Now that we know about DTO and Records, let's compare them in this section.

### Immutability

Records are immutable by design, meaning once you create a record instance, you can't change its data. This immutability ensures that the data remains consistent and thread-safe without needing any extra code. For example, in a `UserRecord`, the fields `name`, `age`, and `email` can be set only when the record is created, and they can't be modified afterward.

On the other hand, DTOs are typically mutable, meaning their fields can be changed after the object is created. To make DTOs immutable, you would have to explicitly avoid setters or design them carefully (e.g., using `final` fields). Here's how immutability looks with a record versus a traditional DTO:

- **Record**: Immutable by default
- **DTO**: Requires manual enforcement for immutability, which can lead to more complex code and potential bugs

```java
public class ImmutabilityExample {
    public static void main(String[] args) {
        UserDTO userDTO = new UserDTO("Ashutosh", 25, "ashutosh@example.com");
        userDTO.setAge(26);  // DTO allows this by default.

        UserRecord userRecord = new UserRecord("Ashutosh", 25, "ashutosh@example.com");
        // userRecord.name = "Jane"; // This would result in a compile-time error.
    }
}
```

### Boilerplate Code

One of the biggest advantages of Records is that they significantly reduce boilerplate code. When using a DTO, you often have to manually write getters, setters, constructors, `equals()`, `hashCode()`, and `toString()` methods. With Records, all of this is generated for you automatically.

In contrast, traditional DTOs require more manual coding. Although tools like Lombok can help reduce the amount of boilerplate, they still don't provide the same level of simplicity as Records. Here's a comparison:

- **Record**: Automatically generates constructor, getters, `equals()`, `hashCode()`, and `toString()`
- **DTO**: Requires manual implementation or the use of tools like Lombok

### Data Representation

Records provide a compact and concise way of representing data. Since the declaration of a Record contains only the fields, the code is cleaner and easier to read. This makes it easier to maintain, especially in projects with a lot of data models.

For example:

```java
// Record: Clean and simple
public record UserRecord(String name, int age, String email) {}
```

Compare this to a DTO, which typically has a lot more code:

```java
// DTO: More verbose
public class UserDTO {
    private String name;
    private int age;
    private String email;

    // Constructor, Getters, Setters, toString, equals, hashCode...
}
```

With Records, the intent is clearer: it's just a data carrier with no extra behavior, whereas DTOs can easily become cluttered with boilerplate or additional logic.

### Customization

One area where DTOs have an advantage is in customization. DTOs allow you to add custom logic, such as data validation, transformation methods, or even business logic if needed (although this is generally discouraged in pure DTOs). For example, you could add a validation method to ensure the email field follows a valid format.

With Records, customization is more limited. Since they are designed to be lightweight and immutable, you can't easily add custom methods that modify internal state or perform complex logic. If your use case requires custom behavior or logic in your data objects, DTOs offer more flexibility.

Here's a quick example of adding custom validation to a DTO:

```java
public class UserDTO {
    private String email;

    // Method to validate email format
    public boolean isValidEmail() {
        return email != null && email.contains("@");
    }
}
```

With a Record, this level of customization would typically be handled outside the Record itself. Records focus strictly on carrying data, while logic like validation is expected to be handled elsewhere.

### Alignment with Functional Programming

One of the key principles of functional programming is immutability — the idea that data objects should not be changed after they are created. Records align more closely with functional programming principles because they are immutable by default. This makes them an ideal choice in systems that favor or adopt a functional programming style.

**Records:**
- Designed to be immutable, which aligns with functional programming's emphasis on creating data structures that cannot change state
- They promote a more declarative style, where you can pass around immutable data objects without side effects, making them predictable and easier to reason about

In contrast, DTOs are traditionally mutable by nature. While it's possible to make DTOs immutable (by avoiding setters and using `final` fields), it requires manual enforcement. DTOs often follow the object-oriented paradigm, where state changes are more common.

**DTOs:**
- More flexible in terms of mutability, which makes them better suited to imperative or object-oriented programming styles
- When used in their mutable form, DTOs can lead to side effects, which is generally discouraged in functional programming

## When to Use DTO vs Record

When deciding between using a DTO or a Record, the choice depends largely on your specific use case, project requirements, and the version of Java you're using. Below is a breakdown of when to use each:

### When to Use DTOs

**When mutability is required:**  
If your object's data needs to be modified after creation, DTOs are the better choice. DTOs are typically mutable, allowing you to change the values of fields as needed. This is useful in scenarios where data is updated throughout the lifecycle of an object.

*Example:* In a web application, a form submission may initially create a `UserDTO` with some fields left blank. As the user updates their profile, the `UserDTO` may need to change accordingly.

**When additional behavior or validation logic is needed:**  
DTOs are more flexible when it comes to adding custom behavior like validation, transformations, or additional methods. If your data object needs logic beyond simply carrying data (e.g., verifying an email format or sanitizing input), then a DTO is more suitable.

*Example:* Adding a method in `UserDTO` to validate the format of an email before passing it between layers of your application.

**Compatibility with older versions of Java (pre-Java 16):**  
If your project is running on a version of Java earlier than Java 16, you won't be able to use Records. In these cases, you'll need to use traditional DTOs or alternatives like Lombok to simplify the code.

*Example:* If your application must support Java 11 or Java 8, then Records are not an option, and you'll stick with DTOs.

### When to Use Records

**When you need a concise, immutable data carrier:**  
Records are ideal when you need a lightweight, immutable object to carry data. Since they automatically generate essential methods (constructor, getters, `equals()`, `hashCode()`, and `toString()`), they offer a clean and efficient way to represent data.

*Example:* If you're transferring data between services in a microservice architecture and don't need to modify the data, a `UserRecord` would be a perfect fit.

**For read-only data transfer between layers or services:**  
If your application involves passing data around without the need to modify it, using a Record is a great choice. The immutability of Records ensures that the data remains consistent, making it suitable for cases like sending data from the database to a service layer or from one service to another.

*Example:* A Record might be used to send user data from a database layer to a REST controller in a web application.

**In modern Java applications (Java 16+):**  
If your project uses Java 16 or later, you can take full advantage of Records. They are designed to simplify data representation in modern Java applications and help reduce the boilerplate that comes with traditional DTOs.

*Example:* In a Java 17 web service, you might use Records for all your data transfer needs between different layers of your application to keep the codebase concise and maintainable.

## Performance Considerations

When comparing DTOs and Records in terms of performance, the differences are typically minimal, but there are a few important factors to consider:

### Memory Efficiency

Since Records are compact by design, they may consume slightly less memory than traditional DTOs. The key reason is that Records do not require the additional overhead of manually implementing getters, setters, `equals()`, `hashCode()`, and `toString()` methods. All of this is generated automatically by the Java compiler in a more optimized way, resulting in a smaller memory footprint.

For example:
- A DTO would need separate fields and methods for each operation (`getName()`, `setName()`, etc.)
- A Record internally holds just the fields and automatically generates the necessary methods, potentially using fewer resources

### Immutability and Thread-Safety

The immutable nature of Records provides some inherent performance benefits, particularly in multi-threaded environments. Since Records are immutable, they don't require synchronization or locking mechanisms when shared between threads. This can lead to better performance in scenarios where thread contention would normally degrade performance.

In contrast, if you use mutable DTOs in multi-threaded environments, you need to ensure thread safety, either by synchronizing access or using other mechanisms, which can introduce overhead and slow down the application.

### Garbage Collection

Both DTOs and Records are plain Java objects (POJOs), so they are subject to the same garbage collection process. However, the concise nature of Records could lead to slightly faster garbage collection, as fewer objects are created or held in memory. This can contribute to improved performance in long-running applications or those handling large volumes of data objects.

### CPU Overhead

Since Records are auto-generated by the compiler and are optimized for performance, there may be slight CPU performance improvements in operations such as object creation, method invocation, and comparison (`equals()`, `hashCode()`). This is particularly true when comparing complex DTOs where developers might introduce inefficiencies in manually implemented methods. The uniformity and optimization of Records ensure that these operations are handled consistently and efficiently.

### Real-World Performance Impact

In practice, the performance differences between DTOs and Records will likely be small and often negligible for most applications. The compact nature of Records might lead to slight performance gains in some scenarios, but the actual impact would only be noticeable in applications with heavy data processing, high throughput, or those running in resource-constrained environments (e.g., mobile or IoT devices).

## Wrapping Up

In this tutorial, we've explored the key differences between DTOs and Records, their respective use cases, and how they align with different programming paradigms like functional programming. While DTOs offer flexibility, mutability, and custom behavior, Records provide a concise and immutable way to model data, making them ideal for modern Java applications.

The decision to use a DTO or a Record ultimately depends on your specific requirements:

- **If you need mutability or want to add custom logic**, DTOs are a better fit
- **If you prefer a compact, immutable structure and are working in Java 16 or later**, Records offer a cleaner and more efficient option

Both approaches have their strengths, and understanding when to use each will help you write more efficient and maintainable Java code.

Source: https://blog.ashutoshkrris.in/dto-vs-record-in-java-which-should-you-use?utm_source=perplexity