---
tags: [java/exceptions, examples, study-guide]
date: 2025-09-04
topic: Java Exceptions Study Guide with Examples
---

<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Java Exceptions Study Guide - Based on Your Course Examples

**Perfect for Test Prep** 🎯

***

## 📋 **Quick Reference from Your Code**

### **Animal Class Exception Types**

```mermaid
graph TD
    A[Animal Class] --> B[Constructor with ParseException - CHECKED]
    A --> C[getName() with NullPointerException - UNCHECKED] 
    A --> D[setAge() with IllegalArgumentException - UNCHECKED]
    A --> E[setVaccinationDate() with ParseException - CHECKED]
    
    B --> F[Must Handle with try-catch or throws]
    C --> G[Optional Handling - Programming Error]
    D --> G
    E --> F
    
    style B fill:#ffcccc
    style E fill:#ffcccc
    style C fill:#ccffcc
    style D fill:#ccffcc
```


***

## 🔴 **Checked Exceptions (From Your Code)**

### **1. ParseException - Date Parsing**

```java
// FROM YOUR Animal.java - Constructor that THROWS checked exception
public Animal(String name, int age, String vaccinationDate) throws ParseException {
    setName(name);
    setAge(age);
    setVaccinationDate(vaccinationDate);  // Can throw ParseException
}

// Method that throws ParseException
public void setVaccinationDate(String dateString) throws ParseException {
    vaccinationDate = new SimpleDateFormat("dd-MM-yyyy").parse(dateString);
    // ⚠️ parse() method throws ParseException - CHECKED!
}
```

> [!WARNING] **Test Alert**
> Any method calling `setVaccinationDate()` MUST handle `ParseException` or declare `throws ParseException`

### **How Your App.java Handles It:**

```java
try {
    myCat.setVaccinationDate("10-05-2015");     // ✅ Correct format
    myDog.setVaccinationDate("10-05-2015");     // ✅ Correct format
    // myDog.setVaccinationDate("15 June, 2018"); // ❌ Wrong format = ParseException
} catch (ParseException e) {
    System.out.println("Invalid date format, should be \"dd-MM-yyyy\"");
} finally {
    System.out.println("The animals are:");
    System.out.println("myDog:" + myDog);
    System.out.println("myCat" + myCat);
}
```


***

## 🟢 **Unchecked Exceptions (From Your Code)**

### **1. NullPointerException - Null Name**

```java
// FROM YOUR Animal.java - Method that THROWS unchecked exception
public String getName() {
    if(name == null) {
        throw new NullPointerException("Name must not be null");  // UNCHECKED!
    }
    return name.toUpperCase();  // This would also throw NPE if name is null
}
```

> [!INFO] **Key Point**
> Your code **manually throws** `NullPointerException` when name is null, rather than letting `name.toUpperCase()` throw it automatically.

### **Safe Version with Exception Handling:**

```java
// FROM YOUR Animal.java - Same logic but with exception handling
public String getName_Handled() {
    try {
        return name.toUpperCase();
    } catch(NullPointerException ex) {
        System.out.println("Caught exception " + ex);
        System.out.println("Message " + ex.getMessage());
        return "No name";  // Safe fallback
    }
}
```


### **2. IllegalArgumentException - Negative Age**

```java
// FROM YOUR Animal.java - Validation with unchecked exception
public void setAge(int newAge) {
    if (newAge < 0) {
        throw new IllegalArgumentException("Age must not be negative: " + newAge);
        // UNCHECKED! - Programming error, caller should validate input
    }
    age = newAge;
}
```


### **3. ArrayIndexOutOfBoundsException - Array Access**

```java
// FROM YOUR App.java - Commented out examples
Integer[] ints = {1,2,3};
System.out.println(ints[^7]); // ArrayIndexOutOfBoundsException - UNCHECKED!
```


### **4. ArithmeticException - Division by Zero**

```java
// FROM YOUR App.java - Commented out examples  
int a = 1 / 0; // ArithmeticException - UNCHECKED!
```


***

## 🛠️ **Exception Handling Patterns from Your Code**

### **Pattern 1: Multi-Catch Block (Your App.java)**

![Exception Flow in Animal Class Example](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/665cc0b929f88b0d1e062469f60b2490/481df3d0-8a22-4b97-b299-4fcf2863f233/ebc0e6ac.png)

Exception Flow in Animal Class Example

```java
try {
    Animal myHamster = new Animal(null, 2, "27-07-2022");        // Could throw multiple exceptions
    Animal myRabbit = new Animal("Vineer", 1, "27-07-2022");
    Animal myOtherDog = new Animal("Dayanna", 2, "27-07-2022");
    Animal myOtherCat = new Animal("Farah", 12, "27-07-2022");

} catch (NullPointerException e) {           // Most specific first
    System.out.println("Name cannot be null");
    
} catch (IllegalArgumentException e) {       // Still specific  
    System.out.println("Age cannot be negative");
    
} catch (ParseException e) {                 // Checked exception
    System.out.println("Date format is incorrect");
    
} catch (RuntimeException e) {               // Broader category
    System.out.println("Caught an RuntimeException object");
    
} catch (Exception e) {                      // Most general last
    System.out.println("Caught an Exception object");
    
} finally {
    System.out.println("Program is complete"); // ALWAYS runs
}
```

> [!TIP] **Catch Block Order Rule**
> **Most specific exceptions first**, **most general last**. Otherwise you get "unreachable code" compilation errors!

### **Pattern 2: Logging Exceptions**

```java
// FROM YOUR App.java - Using Logger for exceptions
public static final Logger LOGGER = Logger.getLogger(com.sparta.nam.logging.App.class.getName());

try {
    throw new RuntimeException();
} catch (Exception ex) {
    LOGGER.log(Level.SEVERE, "SEVERE Name cannot be null"); // Log the error
}
```


***

## 📊 **Your Code Examples Summary Table**

| **Exception Type** | **Class** | **Checked?** | **When Thrown** | **Must Handle?** |
| :-- | :-- | :-- | :-- | :-- |
| `ParseException` | Animal | ✅ **YES** | Invalid date format | **YES** - try/catch or throws |
| `NullPointerException` | Animal | ❌ **NO** | Name is null | **NO** - but should fix code |
| `IllegalArgumentException` | Animal | ❌ **NO** | Negative age | **NO** - but should validate input |
| `ArrayIndexOutOfBoundsException` | App | ❌ **NO** | Array index > length | **NO** - but should check bounds |
| `ArithmeticException` | App | ❌ **NO** | Division by zero | **NO** - but should check divisor |


***

## 🎯 **Test Questions Based on Your Code**

### **Question 1: Compilation**

```java
// Will this compile?
public void testAnimal() {
    Animal dog = new Animal("Rex", 5, "15-08-2023");  // ❌ NO!
}
```

**Answer**: ❌ **Compilation Error** - `ParseException` is checked and not handled

**Fixed versions**:

```java
// Option A: try-catch
public void testAnimal() {
    try {
        Animal dog = new Animal("Rex", 5, "15-08-2023");
    } catch (ParseException e) {
        e.printStackTrace();
    }
}

// Option B: throws declaration
public void testAnimal() throws ParseException {
    Animal dog = new Animal("Rex", 5, "15-08-2023");
}
```


### **Question 2: Runtime Behavior**

```java
Animal cat = new Animal();  // name is null
cat.getName();  // What happens?
```

**Answer**: ✅ **Compiles fine**, throws `NullPointerException` at **runtime**

### **Question 3: Exception Order**

**Which catch block catches `IllegalArgumentException`?**

```java
try {
    animal.setAge(-5);
} catch (RuntimeException e) { /* Block 1 */ }
  catch (IllegalArgumentException e) { /* Block 2 */ }  // ❌ Unreachable!
```

**Answer**: **Block 1** - `IllegalArgumentException` extends `RuntimeException`, so the first catch block handles it. Block 2 is unreachable code!

***

## 💡 **Memory Tricks from Your Examples**

### **CHECKED = "Parse Problems"**

- **P**arseException (dates)
- **P**roblems with external data
- **P**rogrammer must handle


### **UNCHECKED = "NAIL Problems"**

- **N**ullPointerException (forgot to check null)
- **A**rithmeticException (division by zero)
- **I**llegalArgumentException (bad method parameters)
- **L**ength exceptions (array bounds)


### **Exception Handling Order: "S.G.L."**

- **S**pecific exceptions first
- **G**eneral exceptions last
- **L**ogging in finally block

***

## ✅ **Quick Self-Test with Your Code**

1. **What exception does `new SimpleDateFormat("dd-MM-yyyy").parse("bad date")` throw?**
    - ✅ `ParseException` (checked)
2. **What happens if you call `getName()` on an Animal with null name?**
    - ✅ Throws `NullPointerException` (unchecked)
3. **Must you handle the exception from `setAge(-1)`?**
    - ❌ No - `IllegalArgumentException` is unchecked
4. **Which runs even if an exception occurs?**
    - ✅ `finally` block
5. **Can you catch `IllegalArgumentException` after `RuntimeException`?**
    - ❌ No - unreachable code compilation error

***

## 🏷️ **Study Tags**

\#java/exceptions \#java/parseexception \#java/nullpointerexception \#java/illegalargumentexception \#java/animal-class \#sparta-training

> [!EXAMPLE] **Practice Exercise**
> Try modifying the Animal constructor to validate the name parameter and throw appropriate exceptions. Then update App.java to handle all possible exceptions!

**Good luck on your test!** 🍀
<span style="display:none">[^1][^2]</span>

<div style="text-align: center">⁂</div>

[^1]: Animal.java

[^2]: App.java
