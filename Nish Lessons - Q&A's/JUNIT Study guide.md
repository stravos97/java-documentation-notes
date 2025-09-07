---
tags: [java, junit, testing, study-guide, exam-prep]
date: 2025-09-04
topic: JUnit Test Preparation Guide
---
## Part 1: Core JUnit 5 Annotations

### Essential Test Annotations

#### `@Test`
- **Purpose**: Marks a method as a test case
- **Requirements**: Method must be `void` and take no parameters
- **Example**:
```java
@Test
void shouldCalculateCorrectSum() {
    assertEquals(5, calculator.add(2, 3));
}
```

#### `@DisplayName`
- **Purpose**: Provides readable test names
- **Best Practice**: Use descriptive names for test reports
- **Example**:
```java
@DisplayName("Should return true for even numbers")
@Test
void evenNumberTest() { /* test code */ }
```

### Test Lifecycle Annotations

#### `@BeforeAll` / `@AfterAll`
- **When**: Execute **once** per test class
- **Requirement**: Must be **static** methods
- **Use Cases**: Database connections, expensive setup/cleanup
```java
@BeforeAll
static void setupDatabase() {
    // Runs once before all tests
}

@AfterAll
static void cleanup() {
    // Runs once after all tests
}
```

#### `@BeforeEach` / `@AfterEach`  
- **When**: Execute **before/after each** test method
- **Requirement**: Instance methods (non-static)
- **Use Cases**: Fresh object creation, test isolation
```java
@BeforeEach
void setUp() {
    calculator = new Calculator(); // Fresh instance per test
}

@AfterEach  
void tearDown() {
    // Clean up after each test
}
```

> **EXAM TIP**: Remember the execution order!

---

## Part 2: Test Execution Lifecycle

### Execution Order (MEMORIZE THIS!)

```
1. @BeforeAll (static, once)
2. Constructor (per test)
3. @BeforeEach (per test)
4. @Test method
5. @AfterEach (per test)
6. @AfterAll (static, once)
```

### Static vs Instance Fields

#### Instance Fields (Recommended)
```java
class UserTests {
    private User user; // Fresh per test
    
    @BeforeEach
    void setUp() {
        user = new User("John");
    }
}
```

#### Static Fields (Use Carefully)
```java
class DatabaseTests {
    private static Database db; // Shared across tests
    
    @BeforeAll
    static void setup() {
        db = new Database();
    }
}
```

> **EXAM WARNING**: Static fields maintain state between tests - can cause test interference!

---

## Part 3: Parameterized Testing

### `@ParameterizedTest`
- **Purpose**: Run same test with different data
- **Must use**: Parameter sources (`@ValueSource`, `@CsvSource`, etc.)

#### `@ValueSource`
- **For**: Single parameter tests
- **Types**: `ints`, `strings`, `doubles`, `booleans`, etc.
```java
@ParameterizedTest
@ValueSource(ints = {2, 4, 6, 8})
void isEven_ReturnsTrue_ForEvenNumbers(int number) {
    assertTrue(MathUtils.isEven(number));
}
```

#### `@CsvSource`
- **For**: Multiple parameters
- **Format**: Comma-separated values
```java
@ParameterizedTest
@CsvSource({
    "1, 1, 2",
    "2, 3, 5", 
    "5, 7, 12"
})
void add_ReturnsCorrectSum(int a, int b, int expectedSum) {
    assertEquals(expectedSum, calculator.add(a, b));
}
```

> **EXAM TIP**: Parameters map to method parameters in order!

---

## Part 4: Exception Testing

### `assertThrows()`
- **Purpose**: Test that methods throw expected exceptions
- **Returns**: The thrown exception for further assertions
- **Syntax**: `assertThrows(ExceptionType.class, () -> methodCall())`

```java
@Test
void setAge_ThrowsException_WhenNegative() {
    Animal animal = new Animal();
    
    IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class,
        () -> animal.setAge(-5)
    );
    
    assertEquals("Age must not be negative: -5", exception.getMessage());
}
```

### Key Points for Exams:
- Use **lambda expressions**: `() -> methodCall()`
- Can verify **exception messages**
- **Returns the exception** for additional assertions

---

## Part 5: Hamcrest Matchers

### Why Use Hamcrest?
- More **readable** assertions
- Better **error messages**
- More **expressive** than standard JUnit

### Essential Import
```java
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;
```

### Core Matchers

#### Basic Matchers
```java
assertThat(value, is(equalTo(expected)));
assertThat(value, is(expected)); // shorthand
assertThat(value, not(nullValue()));
assertThat(value, notNullValue()); // shorthand
```

#### String Matchers
```java
assertThat(text, startsWith("Hello"));
assertThat(text, endsWith("World"));
assertThat(text, containsString("test"));
assertThat(text, containsStringIgnoringCase("TEST"));
```

#### Collection Matchers
```java
assertThat(list, hasSize(5));
assertThat(list, hasItem("apple"));
assertThat(list, hasItems("apple", "banana"));
assertThat(list, containsInAnyOrder("c", "b", "a"));
assertThat(list, not(hasItem("grape")));
```

#### Combining Matchers
```java
// ALL conditions must be true
assertThat(value, allOf(
    startsWith("Hello"),
    endsWith("World")
));

// ANY condition can be true
assertThat(value, anyOf(
    equalTo("Hi"),
    equalTo("Hello")
));
```

---

## Part 6: Test Execution Order

### `@Order` Annotation
```java
@TestMethodOrder(OrderAnnotation.class)
class OrderedTests {
    
    @Test
    @Order(1)
    void firstTest() { /* runs first */ }
    
    @Test
    @Order(2) 
    void secondTest() { /* runs second */ }
}
```

> **EXAM WARNING**: Tests should be **independent**! Use `@Order` sparingly.

---

## Part 7: Common Assertions Quick Reference

### JUnit 5 Standard Assertions
```java
// Equality
assertEquals(expected, actual);
assertEquals(expected, actual, delta); // for doubles
assertNotEquals(unexpected, actual);

// Boolean
assertTrue(condition);
assertFalse(condition);

// Null checks
assertNull(object);
assertNotNull(object);

// Arrays/Collections
assertArrayEquals(expectedArray, actualArray);

// Exceptions
assertThrows(ExceptionType.class, () -> methodCall());
assertDoesNotThrow(() -> methodCall());
```

### JUnit vs Hamcrest Comparison

| **JUnit** | **Hamcrest** | **Notes** |
|-----------|--------------|-----------|
| `assertEquals(a, b)` | `assertThat(b, equalTo(a))` | Order difference! |
| `assertTrue(x)` | `assertThat(x, is(true))` | More readable |
| `assertNotNull(x)` | `assertThat(x, notNullValue())` | Better error messages |

---

## Part 8: Best Practices & Common Patterns

### Test Method Naming
1. **`methodName_stateUnderTest_expectedBehavior`**
   ```java
   void calculateDiscount_ValidCustomer_ReturnsDiscountedPrice()
   ```

2. **`should_expectedBehavior_when_stateUnderTest`**
   ```java
   void should_ReturnTrue_when_UserIsActive()
   ```

### Arrange-Act-Assert Pattern
```java
@Test
void calculateTotal_WithDiscount_ReturnsCorrectAmount() {
    // Arrange
    Calculator calc = new Calculator();
    
    // Act
    double result = calc.calculateWithDiscount(100, 0.1);
    
    // Assert
    assertEquals(90.0, result);
}
```

### System Under Test (SUT) Pattern
```java
class CalculatorTests {
    private Calculator sut; // System Under Test
    
    @BeforeEach
    void setUp() {
        sut = new Calculator();
    }
    
    @Test
    void add_ReturnsCorrectSum() {
        assertEquals(8.0, sut.add(3, 5));
    }
}
```

---

## Part 9: Sample Test Questions & Answers

### Question 1: Lifecycle Methods
**Q**: In what order do JUnit 5 lifecycle methods execute?

**A**: 
1. `@BeforeAll` (static, once)
2. Constructor (per test)
3. `@BeforeEach` (per test)
4. `@Test` method
5. `@AfterEach` (per test)
6. `@AfterAll` (static, once)

### Question 2: Static Requirements
**Q**: Which annotations require static methods?

**A**: `@BeforeAll` and `@AfterAll` must be static (unless using `@TestInstance(PER_CLASS)`)

### Question 3: Exception Testing
**Q**: How do you test that a method throws a specific exception?

**A**: 
```java
assertThrows(IllegalArgumentException.class, () -> method.call());
```

### Question 4: Parameterized Tests
**Q**: What's the difference between `@ValueSource` and `@CsvSource`?

**A**: 
- `@ValueSource`: Single parameter per test
- `@CsvSource`: Multiple parameters per test (comma-separated)

### Question 5: Hamcrest Benefits
**Q**: Why use Hamcrest over standard JUnit assertions?

**A**: 
- More readable code
- Better error messages
- More expressive matchers
- Easier to combine conditions

---

## Part 10: Code Analysis - Your Examples

### From Your Counter Tests
```java
@BeforeAll
static void setUpAll(){
    sut = new Counter(6);
}

@Test
@Order(1)
void decrement_DecrementCountByOne(){
    sut.decrement();
    Assertions.assertEquals(5, sut.getCount());
}
```

**Key Points**:
- Uses **static field** with `@BeforeAll`
- Uses **`@Order`** for test sequence
- Tests are **dependent** on execution order

### From Your Exception Tests
```java
@Test
@DisplayName("Given an age below zero, setAge throws an IllegalArgumentException")
public void setAgeSadPath() {
    Animal sut = new Animal();
    var exception = Assertions.assertThrows(
        IllegalArgumentException.class, 
        () -> sut.setAge(-2)
    );
    Assertions.assertEquals("Age must not be negative: -2", exception.getMessage());
}
```

**Key Points**:
- Proper **exception testing**
- Validates **exception message**
- Good **method naming**

### From Your Hamcrest Tests
```java
@Test
void given2And6_Add_Returns8Pt0() {
    Calculator calc = new Calculator(6, 2);
    
    Assertions.assertEquals(8.0, calc.add());
    
    assertThat(calc.add(), is(8.0));
    assertThat(calc.add(), is(equalTo(8.0)));
    assertThat(calc.add(), equalTo(8.0));
}
```

**Key Points**:
- Shows **JUnit vs Hamcrest** comparison
- Multiple **equivalent assertions**
- **`is()`** wrapper for readability

---

## Part 11: Common Exam Mistakes to Avoid

### ❌ **Mistake 1**: Wrong Static Usage
```java
// WRONG - @BeforeEach with static
@BeforeEach
static void setUp() { } // Compilation error!
```

### ❌ **Mistake 2**: Parameter Order Confusion
```java
// WRONG - expected vs actual confusion
assertEquals(actual, expected); // backwards!

// CORRECT
assertEquals(expected, actual);
```

### ❌ **Mistake 3**: Missing Lambda in assertThrows
```java
// WRONG
assertThrows(Exception.class, methodCall()); // Direct call!

// CORRECT  
assertThrows(Exception.class, () -> methodCall()); // Lambda
```

### ❌ **Mistake 4**: Forgetting @ParameterizedTest
```java
// WRONG
@Test // Should be @ParameterizedTest
@ValueSource(ints = {1, 2, 3})
void testNumbers(int number) { }
```

### ❌ **Mistake 5**: Wrong Hamcrest Import
```java
// WRONG - JUnit 5 doesn't have assertThat
import static org.junit.jupiter.api.Assertions.assertThat;

// CORRECT
import static org.hamcrest.MatcherAssert.assertThat;
```

---

## Part 12: Final Checklist

Before your test, make sure you can:

### ✅ Core Concepts
- [ ] Explain the purpose of each lifecycle annotation
- [ ] Recite the execution order from memory
- [ ] Identify when to use static vs instance methods
- [ ] Write basic `@Test` methods

### ✅ Parameterized Testing
- [ ] Use `@ValueSource` for single parameters
- [ ] Use `@CsvSource` for multiple parameters
- [ ] Map parameters to method arguments correctly

### ✅ Exception Testing
- [ ] Write `assertThrows()` with proper lambda syntax
- [ ] Verify exception messages
- [ ] Understand return value of `assertThrows()`

### ✅ Hamcrest Matchers
- [ ] Use basic matchers (`is`, `equalTo`, `not`)
- [ ] Apply string matchers (`startsWith`, `contains`)
- [ ] Apply collection matchers (`hasSize`, `hasItem`)
- [ ] Combine matchers with `allOf`/`anyOf`

### ✅ Best Practices
- [ ] Use proper test naming conventions
- [ ] Apply Arrange-Act-Assert pattern
- [ ] Identify test independence issues
- [ ] Choose appropriate assertion styles

---

## Part 13: Quick Reference Cards

### Lifecycle Execution Order
```
@BeforeAll (static, once)
    ↓
Constructor (per test)
    ↓  
@BeforeEach (per test)
    ↓
@Test method
    ↓
@AfterEach (per test)
    ↓
@AfterAll (static, once)
```

### Essential Imports
```java
// JUnit Core
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

// Parameterized
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.*;

// Hamcrest
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;
```

### Common Assertions
```java
// Equality
assertEquals(expected, actual);
assertThat(actual, equalTo(expected));

// Booleans  
assertTrue(condition);
assertThat(condition, is(true));

// Exceptions
assertThrows(Exception.class, () -> method());

// Collections
assertThat(list, hasSize(3));
assertThat(list, hasItem("value"));
```
