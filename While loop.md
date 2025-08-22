## Java While Loop: Number Guessing Game

> [!INFO]  
> While loops in Java are ideal for repeating code when the number of iterations is unknown upfront. They continue executing as long as a condition remains true. Syntax: `while (condition) { // code }`.  
> This makes them perfect for interactive scenarios like games where user input determines when to stop.

Let's build a simple **number guessing game** using a while loop. The program generates a random number between 1 and 100, and the user guesses until they get it right. We'll use `java.util.Random` for number generation and `java.util.Scanner` for input.

## Game Requirements

- Generate a random number (1-100).
    
- Prompt the user to guess.
    
- Provide feedback: "Too high", "Too low", or "Correct!".
    
- Count the number of attempts.
    
- Loop until the guess is correct.
    

> [!TIP]  
> Use while loops for user-driven interactions. Always include a way to exit the loop to avoid infinite loops.

## Code Example

```java
import java.util.Random;
import java.util.Scanner;

public class NumberGuessingGame {
    public static void main(String[] args) {
        // Create Random and Scanner objects
        Random random = new Random();
        Scanner scanner = new Scanner(System.in);
        
        // Generate random number between 1 and 100
        int targetNumber = random.nextInt(100) + 1;
        int guess = 0;  // Initialize guess variable
        int attempts = 0;  // Track number of attempts
        
        System.out.println("Welcome to the Number Guessing Game!");
        System.out.println("I've picked a number between 1 and 100. Try to guess it!");
        
        // While loop: Continues until guess matches target
        while (guess != targetNumber) {
            System.out.print("Enter your guess: ");
            guess = scanner.nextInt();  // Read user input
            attempts++;  // Increment attempt counter
            
            if (guess < targetNumber) {
                System.out.println("Too low! Try again.");
            } else if (guess > targetNumber) {
                System.out.println("Too high! Try again.");
            } else {
                System.out.println("Congratulations! You guessed it in " + attempts + " attempts.");
            }
        }
        
        // Close scanner to prevent resource leak
        scanner.close();
    }
}

```

> [!EXAMPLE]  
> Sample Output:  
> Welcome to the Number Guessing Game!  
> I've picked a number between 1 and 100. Try to guess it!  
> Enter your guess: 50  
> Too low! Try again.  
> Enter your guess: 75  
> Too high! Try again.  
> Enter your guess: 62  
> Congratulations! You guessed it in 3 attempts.

## Explanation

- **Initialization**: We set up `Random` for the target number and `Scanner` for input.
    
- **While Loop**: The loop runs while `guess != targetNumber`. It reads input, checks the guess, and provides feedback.
    
- **Condition Check**: Inside the loop, if-else statements guide the user.
    
- **Exit**: The loop ends naturally when the guess is correct.
    

> [!WARNING]  
> Always validate user input! In a real app, add checks for non-integer inputs using `scanner.hasNextInt()` to avoid exceptions.

## Enhancements and Variations

- **Add Guess Limits**: Modify the condition to `while (guess != targetNumber && attempts < 10)` for a limited-attempts version.
    
- **Difficulty Levels**: Adjust the random range based on user choice.
    
- **Related Concepts**: Combine with [[Java Switch Statement]] for menu options or [[Java Exceptions]] for input handling.
    

> [!NOTE]  
> This example uses Java 8+ features. For older versions, import and usage remain similar.

For more on loops, see [[Java Loops]].

#java #loops #while-loop #games #programming-example



## Java Program: Prime Number Finder Explained

> [!INFO]  
> This Java program finds and prints the first N prime numbers, where N is user-specified. It uses a `while` loop to iterate through numbers starting from 2, checking each for primality with an optimized `for` loop. This is a practical example of nested loops, conditional checks, and basic input handling in Java.

The code demonstrates efficient prime checking by only testing divisors up to the square root of the number, reducing unnecessary computations. It's a great starting point for understanding number theory in programming.

> [!NOTE]  
> Prime numbers are integers greater than 1 with no positive divisors other than 1 and themselves. This program starts from 2 (the smallest prime) and generates primes sequentially.

## Program Overview

- **Purpose**: Generate and display the first `maxPrimes` prime numbers based on user input.
    
- **Key Concepts**: [[Java Loops]] (while and for), conditional statements, [[Java Math]] methods like `Math.sqrt()`, and user input via `Scanner`.
    
- **Efficiency**: The primality test is optimized to O(sqrt(n)) time complexity per number, making it suitable for small to moderate values of N.
    
- **When to Use**: Ideal for educational purposes, coding interviews, or as a building block for algorithms like the Sieve of Eratosthenes (for larger ranges).
    
- **Version Note**: Compatible with Java 8+; no version-specific features are used.
    

> [!TIP]  
> For larger prime generation, consider the ==Sieve of Eratosthenes== algorithm for better performance, as this trial division method scales poorly for big N.

## Code Breakdown

Let's dissect the code step by step. I'll include the full code first for reference, then explain each section.

```java
import java.util.Scanner;

public class PrimeNumberFinder {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int count = 0;
        int number = 2;

        System.out.print("How many prime numbers do you want to find? ");
        int maxPrimes = scanner.nextInt();

        System.out.println("First " + maxPrimes + " prime numbers:");

        while (count < maxPrimes) {
            boolean isPrime = true;
        
            for (int i = 2; i <= Math.sqrt(number); i++) {
                if (number % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        
            if (isPrime) {
                System.out.print(number + " ");
                count++;
            }
        
            number++;
        }

        scanner.close();
    }
}

```

## 1. Import Statement

```java
import java.util.Scanner;
```

- This imports the `Scanner` class from Java's utility package, allowing the program to read user input from the console (System.in).
    
- **Why?** Without this, you couldn't use `Scanner` for interactive input. Alternatives include `BufferedReader` for more advanced I/O, but `Scanner` is simpler for beginners.
    

## 2. Class and Main Method

```java
public class PrimeNumberFinder {
    public static void main(String[] args) {
        // Code here
    }
}
```

- Defines a public class named `PrimeNumberFinder`. In Java, the file name should match the class name (PrimeNumberFinder.java).
    
- The `main` method is the entry point. `String[] args` allows command-line arguments (unused here).
    
- **Best Practice**: Always keep the main method clean; consider extracting logic to separate methods for larger programs.
    

## 3. Initialization and User Input

```java
Scanner scanner = new Scanner(System.in);
int count = 0;
int number = 2;

System.out.print("How many prime numbers do you want to find? ");
int maxPrimes = scanner.nextInt();

System.out.println("First " + maxPrimes + " prime numbers:");

```

- Creates a `Scanner` object to read from standard input.
    
- `count` tracks how many primes have been found (starts at 0).
    
- `number` starts at 2, the first prime, and will increment to check subsequent numbers.
    
- Prompts the user and reads an integer with `scanner.nextInt()` into `maxPrimes`.
    
- Prints a header message using string concatenation.
    
- **Common Pitfall**: If the user enters non-integer input, it throws an `InputMismatchException`. Add try-catch for robustness (see Enhancements below).
    

> [!WARNING]  
> Avoid infinite loops by ensuring the loop condition (e.g., `count < maxPrimes`) can eventually become false. Here, `number++` and `count++` ensure progress.

## 4. The While Loop: Finding Primes

```java
while (count < maxPrimes) {
    boolean isPrime = true;
    
    for (int i = 2; i <= Math.sqrt(number); i++) {
        if (number % i == 0) {
            isPrime = false;
            break;
        }
    }
    
    if (isPrime) {
        System.out.print(number + " ");
        count++;
    }
    
    number++;
}

```

- **Outer While Loop**: Continues until `count` reaches `maxPrimes`. This is perfect for unknown iteration counts (we don't know how many numbers we'll check to find N primes).
    
- **Primality Check**:
    
    - Assume `isPrime` is true initially.
        
    - Inner `for` loop checks divisors from 2 up to `Math.sqrt(number)` (inclusive). `Math.sqrt()` returns a double, but the loop treats it as int via casting in the condition.
        
    - If `number % i == 0`, it's not prime: set `isPrime = false` and `break` to exit early.
        
- **If Prime**:
    
    - Print the number (with a space) and increment `count`.
        
- Always increment `number` to check the next candidate.
    
- **Optimization Note**: Checking up to sqrt(n) is efficient because if n has a factor larger than its sqrt, it must also have one smaller (already checked). For example, for 25, sqrt=5, and it checks 2-5.
    
- **Performance**: For small `maxPrimes` (e.g., 100), it's fast. For large values (e.g., 1,000,000), it could be slow—consider alternatives.
    

> [!TIP]  
> Use `int sqrtLimit = (int) Math.sqrt(number);` and loop `i <= sqrtLimit` for slight clarity and to avoid repeated sqrt calls, though modern JVMs optimize this.

## Why Start at 2?

**Prime Definition:** A prime number has exactly two divisors: 1 and itself.

- **Why not start at 0?** Division by 0 is undefined/causes an error
- **Why not start at 1?** Every number is divisible by 1, so checking `number % 1` would always equal 0 and incorrectly mark all numbers as "not prime"
- **Start at 2:** This is the first meaningful divisor to check. If a number is divisible by anything between 2 and √number, it's composite

## The Logic

The algorithm essentially asks: "Does this number have any divisors OTHER than 1 and itself?"

- Start checking at 2 (the smallest possible "other" divisor)
- End at √number (mathematical optimization)
- If we find any divisor in this range, the number is composite
- If we find no divisors in this range, the number is prime

Starting at `i = 2` ensures we only check meaningful divisors that actually determine primality.

## 5. Closing the Scanner

```java
scanner.close();
```

- Releases system resources associated with the scanner. Good practice to prevent leaks, especially in larger apps.
    

## How It Works: Step-by-Step Execution

Let's trace through finding the **first 3 prime numbers** (`maxPrimes = 3`):

#### Initial State

| Variable    | Value | Purpose                             |
| :---------- | :---- | :---------------------------------- |
| `count`     | 0     | No primes found yet                 |
| `number`    | 2     | Starting with first prime candidate |
| `maxPrimes` | 3     | User wants 3 primes                 |

> [!EXAMPLE]
> **User Input:** 3
> **Expected Output:** 2 3 5

#### Iteration-by-Iteration Analysis

**Iteration 1: Testing number = 2**

```java
// Loop condition check
while (count < maxPrimes)  // 0 < 3 → true, continue

// Prime test for 2
boolean isPrime = true;
for (int i = 2; i <= Math.sqrt(2); i++) {  // i <= 1.4, loop doesn't run
    // No iterations, isPrime stays true
}

if (isPrime) {  // true
    System.out.print(2 + " ");  // Print: "2 "
    count++;  // count becomes 1
}
number++;  // number becomes 3
```

**State after iteration 1:**

- `count = 1`, `number = 3`
- Output so far: `2 `

When a number "skips" the for loop (the loop body never executes), it means **there are no possible divisors to check**, which by definition makes it prime.

The loop skips when: `2 > Math.sqrt(number)`

This happens when: `number < 4`

So for numbers 2 and 3:

- **Number = 2:** `Math.sqrt(2) ≈ 1.41`, condition `2 <= 1.41` is false → loop skips
- **Number = 3:** `Math.sqrt(3) ≈ 1.73`, condition `2 <= 1.73` is false → loop skips

The algorithm logic is: "If I can't find any divisor between 2 and √number, then the number is prime."

For 2 and 3, there literally **aren't any numbers between 2 and √number to check**:

- For 2: No integers exist between 2 and 1.41
- For 3: No integers exist between 2 and 1.73

The algorithm uses "**innocent until proven guilty**" logic:

1. Start by assuming the number IS prime (`isPrime = true`)
2. Try to find evidence it's NOT prime (find a divisor)
3. If no evidence is found, the assumption stands

***

**Iteration 2: Testing number = 3**

```java
// Loop condition check
while (count < maxPrimes)  // 1 < 3 → true, continue

// Prime test for 3
boolean isPrime = true;
for (int i = 2; i <= Math.sqrt(3); i++) {  // i <= 1.7, loop doesn't run
    // No iterations, isPrime stays true
}

if (isPrime) {  // true
    System.out.print(3 + " ");  // Print: "3 "
    count++;  // count becomes 2
}
number++;  // number becomes 4
```

**State after iteration 2:**

- `count = 2`, `number = 4`
- Output so far: `2 3 `

***

**Iteration 3: Testing number = 4**

```java
// Loop condition check
while (count < maxPrimes)  // 2 < 3 → true, continue

// Prime test for 4
boolean isPrime = true;
for (int i = 2; i <= Math.sqrt(4); i++) {  // i <= 2.0, i = 2
    if (4 % 2 == 0) {  // 0, divisible!
        isPrime = false;
        break;  // Exit early
    }
}

if (isPrime) {  // false, skip this block
    // Nothing happens - no print, no count increment
}
number++;  // number becomes 5
```

**State after iteration 3:**

- `count = 2` (unchanged), `number = 5`
- Output so far: `2 3 ` (unchanged)

> [!TIP]
> Notice how `count` stays the same when a composite number is found, but `number` always increments. This ensures we test every candidate while only counting actual primes.

***

**Iteration 4: Testing number = 5**

```java
// Loop condition check
while (count < maxPrimes)  // 2 < 3 → true, continue

// Prime test for 5
boolean isPrime = true;
for (int i = 2; i <= Math.sqrt(5); i++) {  // i <= 2.2, i = 2
    if (5 % 2 == 0) {  // 1, not divisible
        // Condition false, continue loop
    }
    // Loop ends, isPrime remains true
}

if (isPrime) {  // true
    System.out.print(5 + " ");  // Print: "5 "
    count++;  // count becomes 3
}
number++;  // number becomes 6
```

**State after iteration 4:**

- `count = 3`, `number = 6`
- Output so far: `2 3 5 `

***

**Loop Termination Check**

```java
while (count < maxPrimes)  // 3 < 3 → false, exit loop
```

> [!SUCCESS]
> **Final Output:** `2 3 5 ` (exactly 3 primes found)

### Key Insights from the Walkthrough

#### 1. Dual Counter Pattern

```java
while (count < maxPrimes) {
    // Test current number
    if (isPrime) {
        count++;  // Only increments for primes
    }
    number++;     // Always increments
}
```

> [!NOTE]
> This pattern separates **progress tracking** (`count`) from **candidate iteration** (`number`). Essential for "find N items matching criteria" algorithms.

#### 2. Optimization: Early Break

```java
for (int i = 2; i <= Math.sqrt(number); i++) {
    if (number % i == 0) {
        isPrime = false;
        break;  // No need to check further
    }
}
```

The `break` statement saves unnecessary iterations once we find a divisor.

#### 3. Mathematical Efficiency

Testing only up to `√number` is mathematically sound:

- If `n = a × b` and both `a, b > √n`, then `a × b > n` (contradiction)
- So at least one factor must be ≤ √n

> [!WARNING]
> **Common Mistake:** Forgetting to increment `number` leads to infinite loops testing the same candidate repeatedly.

### Performance Analysis

| Input Size | Time Complexity | Notes |
| :-- | :-- | :-- |
| Small (N ≤ 100) | Very fast | Suitable for interactive use |
| Medium (N ≤ 1000) | Acceptable | May take a few seconds |
| Large (N > 10000) | Slow | Consider [[Sieve of Eratosthenes]] |

> [!TIP]
> For finding many primes efficiently, pre-compute them using a sieve algorithm, then access by index.

### Related Patterns

This while loop pattern applies to many scenarios:

- Finding N Fibonacci numbers
- Collecting N valid user inputs
- Searching for N files matching criteria
- Generating N random unique values

```java
// General pattern
int count = 0;
int candidate = startValue;
while (count < target) {
    if (meetsCondition(candidate)) {
        processItem(candidate);
        count++;
    }
    candidate++;
}
```


## Common Pitfalls and Improvements

- **Pitfalls**:
    
    - For number=1 or negatives: The code starts at 2, but add checks if needed.
        
    - Large inputs: May run slowly or overflow `int` (primes up to ~2 billion fit in int).
        
    - No handling for maxPrimes <=0: Add validation like `if (maxPrimes <= 0) { System.out.println("Invalid input"); return; }`.
        
- **Enhancements**:
    
    - Handle exceptions: Wrap `nextInt()` in try-catch.
        
    - Even number optimization: After 2, skip evens by incrementing number by 2.
        
    - Make it a method: Extract primality check to a reusable function.
        

**Advanced Example: Optimized Primality Method**

``` java
public static boolean isPrime(int num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 == 0 || num % 3 == 0) return false;
    for (int i = 5; i * i <= num; i += 6) {
        if (num % i == 0 || num % (i + 2) == 0) return false;
    }
    return true;
}

```

This checks multiples of 6 for faster sieving.

For more on number algorithms, see [[Java Math]] or [[Algorithms in Java]].

#java #loops #prime-numbers #algorithms #programming-example