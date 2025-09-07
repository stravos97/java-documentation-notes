---
tags: [java, strings, object, cheatsheet]
date: 2025-08-22
topic: Java, Strings
---

## How is a String an Object in Java?

#java #strings #objects \[[Java Strings]\]

<!-- TOC -->
  * [How is a String an Object in Java?](#how-is-a-string-an-object-in-java)
    * [Quick Reference](#quick-reference)
  * [Why is String an Object?](#why-is-string-an-object)
  * [Internals and Objects](#internals-and-objects)
  * [Practical Examples](#practical-examples)
  * [When is This Important?](#when-is-this-important)
<!-- TOC -->

### Quick Reference


- **A Java `String` is an ==object== and not a primitive!**
- Strings are instances of the `java.lang.String` class, which extends `Object`.
- Every string literal (`"hello"`) is automatically created as a `String` object by the Java compiler and runtime.
- The `String` class stores a sequence of characters and provides many built-in methods for text
  manipulation. [oracle](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [geeksforgeeks](https://www.geeksforgeeks.org/java/java-string-is-immutable-what-exactly-is-the-meaning/)


______________________________________________________________________

## Why is String an Object?


- **Declaration:**


```java
public final class String extends Object
        implements Serializable, Comparable<String>, CharSequence
```


This signature means every Java String has all the properties of a Java object, like methods (`equals()`, `hashCode()`,
etc.), and can be passed around as objects. [Oracle](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)


- **Creation:**
	- **String Literal:**


```java
String s1 = "hello";
```


The Java Virtual Machine (JVM) creates a `String` object and stores it in a special memory area called the ==String
pool==. [Scaler](https://www.scaler.com/topics/java/string-pool-in-java/)


- **Constructor:**


```java
String s2 = new String("hello");
```


Here, a new object is explicitly created in heap memory (not necessarily shared with the
pool). [Javaguides](https://www.javaguides.net/2018/08/java-string-class-api-guide.html)

______________________________________________________________________

## Internals and Objects


- Internally, a String holds a ==character array== (now a byte array since Java 9) and caches its hash code for quick
  use.
- Being an object, a `String` can be
	- Stored in collections: `List<String>`, `Map<String, Integer>`
	- Used with all `Object` methods (`equals()`, `toString()`, etc.)
	- Passed by reference between methods
- ==Immutability==: Once created, a String object's value cannot change. Any operation that appears to modify a string
  returns a new String
  object. [geeksforgeeks](https://www.geeksforgeeks.org/java/java-string-is-immutable-what-exactly-is-the-meaning/) [scaler](https://www.scaler.com/topics/java/string-pool-in-java/)


______________________________________________________________________

## Practical Examples


```java
String name = "Java";            // String literal (object from pool)
String city = new String("NY");  // Explicitly a new String object

System.out.

println(name.length());        // Methods available because String is an object
        System.out.

println(name.toUpperCase());
```


______________________________________________________________________

> [!EXAMPLE]
> - String is used in nearly every Java program as an object: passed to methods, returned from functions, compared using
    > `equals()`, etc.
> - All classes in Java (including String) inherit from the root \[[Object]\] class.

> [!TIP]
> Prefer String literals for most applications for performance and memory efficiency.

> [!NOTE]
> Even though you don't need to write `new` for most Strings, **every String is always an object** in Java.

______________________________________________________________________

## When is This Important?


- String objects ==can be compared== by `==` (reference) or `.equals()` (value).
- Using String methods (e.g., `.substring()`, `.concat()`) is only possible because String is a class, not a primitive.


______________________________________________________________________

#java/strings #java/objects #cheatsheets
