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

## 5. Closing the Scanner

```java
scanner.close();
```

- Releases system resources associated with the scanner. Good practice to prevent leaks, especially in larger apps.
    

## How It Works: Step-by-Step Execution

1. Program starts, asks for input (e.g., user enters 5).
    
2. Sets `maxPrimes=5`, prints header.
    
3. Enters while loop (count=0 < 5).
    
4. Checks if 2 is prime: For loop from 2 to sqrt(2)≈1.41 (loop doesn't run), so isPrime=true. Prints 2, count=1, number=3.
    
5. Repeats: 3 (prime), prints 3, count=2, number=4.
    
6. 4: Divisible by 2, isPrime=false, skips print, number=5.
    
7. Continues until 5 primes found (2,3,5,7,11 for maxPrimes=5).
    

> [!EXAMPLE]  
> Input: 3  
> Output:  
> How many prime numbers do you want to find? 3  
> First 3 prime numbers:  
> 2 3 5

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