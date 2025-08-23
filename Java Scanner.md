______________________________________________________________________

tags: [java, userinput, scanner, bestpractices, java/io]\
date: 2025-08-21\
topic: Java User Input with Scanner

______________________________________________________________________

## Java User Input with Scanner

<!-- TOC -->
  * [Java User Input with Scanner](#java-user-input-with-scanner)
  * [Quick Reference: Importing and Creating Scanner](#quick-reference-importing-and-creating-scanner)
  * [Common Scanner Methods](#common-scanner-methods)
  * [Basic Example: Reading String Input](#basic-example-reading-string-input)
  * [Reading Multiple Inputs and Primitives](#reading-multiple-inputs-and-primitives)
  * [Advanced Usage: Reading from Files or Strings](#advanced-usage-reading-from-files-or-strings)
  * [When to Use Scanner](#when-to-use-scanner)
<!-- TOC -->

The **Scanner** class in Java, found in the `java.util` package, is used to read user input from various sources like
the console, files, or strings. It parses primitive types and strings using methods like `nextLine()` for strings or
`nextInt()` for integers, making it ideal for interactive
programs.[w3schools](https://www.w3schools.com/java/java_user_input.asp)

## Quick Reference: Importing and Creating Scanner


- **Import Statement**: `import java.util.Scanner;` (or `import java.util.*;` for broader
  access).[theserverside](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Java-Scanner-User-Input-example-String-next-int-long-char)

- **Creating Object**: `Scanner scanner = new Scanner(System.in);` to read from console
  input.[stackoverflow](https://stackoverflow.com/questions/5287538/how-to-get-the-user-input-in-java)

- **Closing Scanner**: Always close with `scanner.close();` to free resources, especially in larger
  applications.[programiz](https://www.programiz.com/java-programming/scanner)


> [!TIP]  
> Use a single Scanner instance for multiple inputs in a program to avoid resource

leaks.[theserverside](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Java-Scanner-User-Input-example-String-next-int-long-char)

## Common Scanner Methods


| Method          | Description                       | Return Type | Example Usage                                                                                                                                                                                   |
|-----------------|-----------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `nextLine()`    | Reads a full line of text         | String      | `String name = scanner.nextLine();` [w3schools](https://www.w3schools.com/java/java_user_input.asp)                                                                                             |
| `next()`        | Reads the next token (word)       | String      | `String word = scanner.next();` [theserverside](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Java-Scanner-User-Input-example-String-next-int-long-char)        |
| `nextInt()`     | Reads the next integer            | int         | `int age = scanner.nextInt();` [stackoverflow](https://stackoverflow.com/questions/5287538/how-to-get-the-user-input-in-java)                                                                   |
| `nextDouble()`  | Reads the next double             | double      | `double price = scanner.nextDouble();` [theserverside](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Java-Scanner-User-Input-example-String-next-int-long-char) |
| `nextBoolean()` | Reads the next boolean            | boolean     | `boolean flag = scanner.nextBoolean();` [programiz](https://www.programiz.com/java-programming/scanner)                                                                                         |
| `hasNext()`     | Checks if more input is available | boolean     | `if (scanner.hasNext()) { ... }` [oracle](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)                                                                                     |


> [!NOTE]  
> Methods like `nextInt()` read until a delimiter (default is whitespace), while `nextLine()` reads until newline. For
> Java 17+, these methods support enhanced pattern matching with regular
> expressions.[ Oracle](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)

## Basic Example: Reading String Input


```java
import java.util.Scanner;  // Import Scanner class

public class UserInputExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);  // Create Scanner object for console input

        System.out.print("Enter your name: ");  // Prompt user
        String name = scanner.nextLine();  // Read entire line as string

        System.out.println("Hello, " + name + "!");  // Output the input

        scanner.close();  // Close the scanner to prevent resource leaks
    }
}

```


**Output** (if the user enters "Alice"):\
Hello, Alice![codecademy](https://www.codecademy.com/resources/docs/java/user-input)

> [!EXAMPLE]  
> Real-world use: In a command-line app for collecting user details, like a registration form.

## Reading Multiple Inputs and Primitives

Use a loop or sequential calls for multiple inputs. Handle mixed types carefully to avoid issues with leftover
newlines.[baeldung](https://www.baeldung.com/java-scanner)


```java
import java.util.Scanner;

public class MultiInputExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter your age: ");
        int age = scanner.nextInt();  // Read integer

        scanner.nextLine();  // Consume leftover newline after nextInt()

        System.out.print("Enter your city: ");
        String city = scanner.nextLine();  // Read string

        System.out.println("You are " + age + " years old from " + city + ".");

        scanner.close();
    }
}

```


> [!WARNING]  
> Common pitfall: After `nextInt()`, a newline remains in the buffer, causing `nextLine()` to read an empty string. Fix
> by adding `scanner.nextLine()` to consume it.[baeldung](https://www.baeldung.com/java-scanner)

## Advanced Usage: Reading from Files or Strings


- **From File**:
  `Scanner fileScanner = new Scanner(new File("input.txt"));`.[programiz](https://www.programiz.com/java-programming/scanner)

- **From String**:
  `Scanner strScanner = new Scanner("Hello Java"); String word = strScanner.next();`.[programiz](https://www.programiz.com/java-programming/scanner)


> [!INFO]  
> For performance in large inputs, consider BufferedReader as an alternative, but Scanner is simpler for beginners and
> supports \[[Java Regex]\] for custom
parsing.[ Oracle](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)

## When to Use Scanner


- **Use Cases**: Interactive CLI tools, simple games, or data entry
  forms.[stackoverflow](https://stackoverflow.com/questions/5287538/how-to-get-the-user-input-in-java)

- **Alternatives**: For GUI apps, use `JOptionPane`; for high performance, prefer
  `BufferedReader`.[baeldung](https://www.baeldung.com/java-scanner)

- **Version Note**: Available since Java 5; Java 17+ adds better support for localized
  input.[ Oracle](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)


> [!TIP]  
> Always validate input with try-catch for `InputMismatchException` to handle invalid types, e.g., entering text when
> expecting a number.[programiz](https://www.programiz.com/java-programming/scanner)

> [!EXAMPLE]-
> ```java
> import java.util.Scanner;
> import java.util.InputMismatchException;
>
> public class ErrorHandlingExample {
>     public static void main(String[] args) {
>         Scanner scanner = new Scanner(System.in);
>         try {
>             System.out.print("Enter an integer: ");
>             int number = scanner.nextInt();
>             System.out.println("You entered: " + number);
>         } catch (InputMismatchException e) {
>             System.out.println("Invalid input! Please enter an integer.");
>         } finally {
>             scanner.close();
>         }
>     }
> }
> ```

#java #scanner #userinput #bestpractices #java/io
