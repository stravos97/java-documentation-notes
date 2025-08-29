### Your Answers and My Feedback:


1. **Regarding the`==`operator in Java, how does its behaviour differ when comparing primitive types versus objects?**

	- **Your Answer:**"with primitive types you are just checking if the value is the same, with objects you are
	  comparing memory address and value"
	- **Feedback:**You are**mostly correct**for primitive types. When`==`is used for**primitive types**, it compares
	  their**actual values**. This works for types like`int`,`float`,`char`, and`boolean`, and is considered best
	  practice for comparing primitives. However, for**objects**, your answer is a little off. When`==`compares objects,
	  it**compares their references**(memory addresses). It returns`true`only if both references point to the**exact
	  same object**in memory. Crucially, it**does NOT compare object contents or values**. So, two different objects
	  might have identical content, but`==`would still return`false`because they are not the_same_object. For comparing
	  object_contents\_, you typically use the`equals()`method.
	-

1. **What is the primary reason for using multiple constructors (constructor overloading) in a Java class?**

	- **Your Answer:**"multiple constructors allow you to create an object in different situations. e.g. all the fields
	  being known or defined or some values being know with the others being unknown. you can create the object without
	  placeholders."
	- **Feedback:**This is a**very good answer**and accurately captures the main purpose! The sources confirm that\*
	  *multiple constructors (known as constructor overloading)**allow you to**create objects in different ways*\*,
	  offering**flexibility for initialization**. Key reasons include:
		- **Flexible Object Creation**: You can provide various ways to create an instance, depending on the information
		  available, such as specifying all details, only some, or none.
		- **Convenience & Readability**: This makes code easier to read and use, preventing the need for placeholder
		  arguments, as you mentioned (e.g.,`new Car("Toyota")`instead of`new Car("Toyota", "Unknown", 0)`).
		- **Different Initialization Logic**: Sometimes different constructors perform different setup routines.
		- **Support for Frameworks/Tools**: Many Java frameworks require a no-argument (default) constructor for
		  reflection or serialization.
		-

1. **In Java, is a`String`considered a primitive type or an object?**

	- **Your Answer:**"string is an object"
	- \*\*Feedback:\*\***Exactly right!**A Java`String`is an**==object== and not a primitive**. Strings are instances of
	  the
	  `java.lang.String`class, which extends`Object`. Even when you use a string literal like`"hello"`, it's
	  automatically created as a`String`object by the Java compiler and runtime. Being an object,`String`provides many
	  built-in methods for text manipulation (like`substring()`or`concat()`) and can be used with all standard`Object`
	  methods such as`equals()`.
	-

1. **What are the two main rules for Java class filename matching, especially concerning public versus non-public
   classes within a single`.java`file?**

	- **Your Answer:**"the filename needs to match the class name for a public class (the method has to be public too),
	  a private class doesn't have to abide by this rule"
	- \*\*Feedback:\*\*You've correctly identified the core rules, but let's refine the terminology slightly based on
	  the
	  sources:
		- **Public Classes**: The**filename must match the public class name exactly**(case-sensitive, ending in
		  `.java`). Your point about "the method has to be public too" isn't strictly part of the_filename matching
		  rule_itself for the class, but rather a characteristic of the`main`method within an executable class. The
		  filename rule applies to the class's`public`modifier.
		- **Non-Public Classes**: A single`.java`file can hold multiple classes, but**only one of them can be public
		  **.\*
		  *Non-public classes*\*(which have default access, not necessarily`private`access)**do not require filename
		  matching**. The sources refer to these as "non-public classes (default access)".

1. **What are the key characteristics of the`public static void main(String[] args)`method that make it the entry point
   for a Java application?**

	- **Your Answer:**"not sure"
	- **Feedback:**No worries, this is a very common question, and understanding each part is crucial! The
	  `public static void main(String[] args)`method is the**standard entry point for any standalone Java application**,
	  meaning the Java Virtual Machine (JVM) looks for and begins executing the program from this method. It must be
	  defined**exactly this way**for the JVM to locate and run it.

   Here's a breakdown of each keyword and why it's essential:

	- **`public`**: This is an**access modifier**that makes the method accessible from anywhere. The JVM needs to call
	  `main()`from outside the class, so it must be`public`.
	- **`static`**: This keyword allows the`main()`method to be called\*\*without first creating an instance of the
	  class
	  \*\*. The JVM invokes`main()`before any objects exist, so it must be`static`to be tied to the class itself, not an
	  object.
	- **`void`**: This indicates that the method**does not return any value**. Its purpose is to start the program's
	  execution, not to compute and return a result to the caller.
	- **`main`**: This is the**predefined name**that the JVM specifically looks for as the program's starting point.
	  It's a convention that the JVM requires.
	- **`String[] args`**: This is a parameter that accepts an**array of strings**. It's used for passing**command-line
	  arguments**to the program when it's executed (e.g.,`java MyProgram arg1 arg2`). These arguments allow users to
	  provide input or configuration options directly when running the program.
