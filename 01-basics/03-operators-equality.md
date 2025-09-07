---
tags: [java/operators, equality, primitives, objects]
date: 2025-08-23
topic: == Operator in Java
---

<!-- TOC -->
- [Quick Reference](#quick-reference)
- [How It Works](#how-it-works)
  - [For Primitives](#for-primitives)
  - [For Objects](#for-objects)
  - [For Strings](#for-strings)
- [Operator Table](#operator-table)
- [Use Cases & Best Practices](#use-cases-best-practices)
- [Common Pitfalls](#common-pitfalls)
- [Performance](#performance)
- [Related Concepts](#related-concepts)
<!-- TOC -->


### Quick Reference

- **Type:** Relational operator
- **Result:** Returns a `boolean` (`true` or `false`)
- **Syntax:**


```java
lhs ==rhs
```


______________________________________________________________________

## How It Works

### For Primitives


- Compares **actual values** [oracle](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)
- Example:


```java
int a = 5;
int b = 5;
System.out.println(a == b); // true
```


- Works for: `int`, `float`, `char`, `boolean`, etc.
- **Best practice**: Always use `==` to compare primitive values


### For Objects


- Compares **references** (memory addresses) [baeldung](https://www.baeldung.com/java-equals-method-operator-difference)
- Returns `true` only if both references point to the **exact same object**
- Does NOT compare object contents/values


```java
String s1 = new String("hello");
String s2 = new String("hello");
System.out.println(s1 ==s2); // false

String s3 = s1;
System.out.println(s1 ==s3); // true
```


- **Use cases**: Identity checks (is this literally the same object?)


### For Strings


- Strings with equal contents may not be the same
  object: [oracle](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)


```java
String s1 = "HELLO";
String s2 = "HELLO";
String s3 = new String("HELLO");
System.out.println(s1 == s2); // true (same reference, string pool)
System.out.println(s1 == s3); // false (different objects)
```


- To compare **contents**, use `equals()`:


```java
System.out.println(s1.equals(s3)); // true
```


> [!WARNING]
> Always use `==` for primitive comparison and `equals()` for object content comparison!

______________________________________________________________________

## Operator Table


| Scenario                        | Comparison     | Example                | Returns |
|:--------------------------------|:---------------|:-----------------------|:--------|
| Primitive vs Primitive          | Value          | `5 == 5`               | true    |
| Reference vs Reference (ident.) | Reference      | `a == a`               | true    |
| Reference vs Reference (diff.)  | Reference      | `new A() == new A()`   | false   |
| String Literal Pool             | Reference      | `"A"=="A"`             | true    |
| String (new) vs Literal         | Reference      | `new String("A")=="A"` | false   |
| Object Content                  | (use equals()) | `a.equals(b)`          | varies  |


______________________________________________________________________

## Use Cases & Best Practices

> [!TIP]
> Use `==` for:
>
> - Comparing primitives: `if (count == 10)`
> - Checking if two references are the =same object= in memory

> [!WARNING]
> Don't use `==` for object content comparison (e.g. Strings, custom classes).

> [!EXAMPLE]
> When you want to check if a cached object is reused:
>
> ```java
> if (singletonInstance == anotherReference) { /* ... */ }
> ```

> [!NOTE]
> The `==` operator cannot be overridden in Java (unlike `equals()`).

______________________________________________________________________

## Common Pitfalls


- **Using `==` for strings**: Compares reference, not value! (`"abc" == new String("abc")` is `false`)
- **Comparing objects of incompatible types**: Compile-time error


```java
Object o = new Object();
String s = "test";
System.out.println(o == s); // false, only works if compatible types
```


______________________________________________________________________

## Performance


- **Fast**: Single memory/address or value check
- No function call overhead


______________________________________________________________________

## Related Concepts


- \[[equals() Method]\], \[[hashCode() Method]\], \[[Object Identity]\], \[[Relational Operators]\]


______________________________________________________________________

> [!INFO]
> For custom equality for your own classes, override `equals()`. `==` will always check for identity, not content.

______________________________________________________________________

#java #basics #operators #equality
