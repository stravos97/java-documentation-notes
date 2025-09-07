---
tags: [java, interfaces, keywords, oop, cheatsheet, bestpractices]
date: 2025-08-29
topic: Java interface and implements — “can do” relationship
---

An interface models a capability or “can do” contract, and the implements keyword means a class promises it can do those behaviors by providing method bodies for the interface’s abstract methods. In contrast to class inheritance (“is-a”), implements is about attaching abilities across unrelated types while enabling polymorphism and loose coupling.[^1][^2][^3]

## Core idea: “can do”

An interface defines a set of behaviors a type can perform, commonly described as a CAN-DO relationship (e.g., Drivable means “can drive”) rather than an “is-a” relationship. A class declares implements InterfaceName to agree to the contract and must provide public implementations for all abstract methods unless the class itself is abstract.[^3][^1]

```mermaid
classDiagram
    direction LR
    class Drivable {
        <<interface>>
        +drive()
    }
    class Flyable {
        <<interface>>
        +fly()
    }
    class Car {
        +drive()
    }
    class Drone {
        +fly()
    }
    Drivable <|.. Car
    Flyable <|.. Drone
```


## Syntax basics

- Define an interface with interface; methods are implicitly abstract unless declared default, and fields are implicitly public static final.[^4][^1]
- Implement one or many interfaces in a class: public class X extends Base implements A, B { ... } with implements after extends in the declaration order.[^2][^3]

```java
// Java 17+
// Interface as a "can do" capability
public interface Drivable {
    // implicitly public abstract
    void drive();

    // default method (Java 8+): an opt-in behavior that implementors inherit
    default int maxSpeedKmh() { return 120; }

    // static method exists on the interface type (not inherited)
    static boolean isLegalSpeed(int kmh) { return kmh >= 0; }

    // private helper (Java 9+), usable only inside this interface
    private void log(String msg) { System.out.println("[Drivable] " + msg); }
}

// A class that "can drive"
public class Car implements Drivable {
    @Override // must be public to satisfy the interface contract
    public void drive() { System.out.println("Driving..."); }

    public static void main(String[] args) {
        Drivable d = new Car();
        d.drive();
        System.out.println(d.maxSpeedKmh());
        System.out.println(Drivable.isLegalSpeed(50));
    }
}
```

Each class that implements an interface must publicly implement all abstract methods, or be declared abstract itself, and static interface methods are called via the interface name.[^5][^1][^3]

## Multiple “can do” capabilities

A class can implement multiple interfaces to accumulate capabilities, which is how Java models multiple inheritance of type safely. This makes it straightforward to compose roles like Drivable, Floatable, and Flyable into a single concrete type without class diamond issues.[^1][^5][^2]

```java
interface Floatable { void floatOnWater(); }
interface Flyable { void fly(); }

// AmphibiousPlane "can drive", "can float", and "can fly"
class AmphibiousPlane implements Drivable, Floatable, Flyable {
    @Override public void drive() { System.out.println("Taxi on runway"); }
    @Override public void floatOnWater() { System.out.println("Floating"); }
    @Override public void fly() { System.out.println("Take off"); }
}
```

> [!TIP]
> Prefer small, focused capability interfaces (e.g., Closeable, Comparable) and compose them, which keeps implementations cohesive and testing easier.[^6][^3]

## Default, static, and private methods

- Default methods (Java 8+) let interfaces evolve behavior without breaking implementors; implementors inherit them but can override when needed.[^7][^1]
- Static methods (Java 8+) are utilities on the interface type and are not inherited by implementing classes; call them as InterfaceName.method().[^5][^1]
- Private interface methods (Java 9+) encapsulate reusable code for default/static methods within the interface body.[^8]

```java
interface Trackable {
    default String id() { return generateId(); } // default uses a private helper
    private String generateId() { return "ID-" + System.nanoTime(); } // Java 9+
    static boolean valid(String id) { return id != null && id.startsWith("ID-"); } // Java 8+
}
```

Default and private interface methods enable API evolution with shared behavior while keeping the public surface area stable and consistent.[^8][^1]

## Conflict rules for defaults

When multiple inherited defaults collide, Java applies rules to resolve ambiguity, and sometimes requires an explicit override choosing InterfaceName.super.method(). The precedence is: classes win over interfaces; more specific subinterfaces win over supertypes; otherwise the class must override and can delegate to a chosen parent default.[^9][^10][^11]

```java
interface A { default void ping() { System.out.println("A"); } }
interface B { default void ping() { System.out.println("B"); } }

class C implements A, B {
    @Override
    public void ping() {
        // Must disambiguate or provide own behavior
        A.super.ping(); // explicit choice of A's default
    }
}
```

These rules avoid the classic diamond problem while keeping “can do” composition practical and predictable in real systems.[^10][^11]

## Polymorphism: program to capabilities

Code often depends on interfaces rather than concrete classes so that any type that “can do” the contract can be substituted, improving testability and flexibility. This enables swapping implementations (e.g., mock vs. real) without changing callers as long as the capability contract remains stable.[^12][^3]

```java
interface PaymentProcessor { void charge(long cents); }

class StripeProcessor implements PaymentProcessor {
    @Override public void charge(long cents) { /* call Stripe API */ }
}
class FakeProcessor implements PaymentProcessor {
    @Override public void charge(long cents) { /* test no-op */ }
}

class CheckoutService {
    private final PaymentProcessor processor;
    CheckoutService(PaymentProcessor processor) { this.processor = processor; }
    void checkout(long cents) { processor.charge(cents); }
}
```

Depending on PaymentProcessor allows injecting any object that “can process payments” at runtime, which is the essence of the “can do” abstraction with interfaces.[^3][^12]

## Extends vs implements (concept)

- Extends models “is-a” between classes, or interface-to-interface refinement, with single inheritance for classes to avoid ambiguity.[^2]
- Implements models “can do” capabilities for classes and supports multiple capabilities on one class via multiple interface implementation.[^1][^2]

```mermaid
classDiagram
    direction LR
    class Vehicle
    class Car
    Vehicle <|-- Car : "is-a" (extends)

    class Insurable {
        <<interface>>
        +premium()
    }
    class Rentable {
        <<interface>>
        +rent()
    }
    Insurable <|.. Car : "can do" (implements)
    Rentable <|.. Car : "can do" (implements)
```


## Extends vs implements (quick table)

| Aspect | extends | implements |
| :-- | :-- | :-- |
| Relationship | “is-a” between classes or interface-to-interface refinement [^2] | “can do” capabilities added to classes [^1] |
| Count | Class extends one class; interface may extend multiple interfaces [^2] | Class may implement many interfaces [^2] |
| Obligation | Subclass may override some methods; not all required [^2] | Must implement all abstract methods unless class is abstract [^2] |
| Syntax order | class C extends Base { } [^2] | class C extends Base implements A,B { } [^3] |

## Best practices and pitfalls

- Always use @Override and keep implementations public to satisfy the interface’s public contract.[^1]
- Name capability interfaces by ability (e.g., Drivable, Measurable), and keep them small for interface segregation and easy testing.[^6][^3]
- Remember static interface methods are not inherited; call them with InterfaceName.method(), not via an instance.[^5][^1]

> [!WARNING]
> If two interfaces provide the same default method, the class must override and decide, optionally delegating with InterfaceName.super.method() to resolve ambiguity.[^11][^10]

## Quick reference

- Define capability: interface Cap { void doIt(); default void log(){} }.[^7][^1]
- Implement capability: class X implements Cap { public void doIt(){} }.[^2][^3]
- Multiple capabilities: class X implements A, B { ... }.[^2][^1]
- Private helpers in interfaces (Java 9+): private void helper() { ... }.[^8]

```mermaid
sequenceDiagram
    participant Client
    participant Capability as Interface Capability
    participant Impl as ConcreteImpl
    Client->>Capability: call doWork() via reference
    Note right of Capability: Polymorphic call contract [“can do”] [^22]
    Capability->>Impl: dynamic dispatch to implementation
    Impl-->>Client: result
```

\#java \#java/interfaces \#java/oop \#keywords \#bestpractices[^10][^3][^1][^2]


[^1]: https://www.geeksforgeeks.org/java/interfaces-in-java/

[^2]: https://www.geeksforgeeks.org/java/extends-vs-implements-in-java/

[^3]: https://docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html

[^4]: https://www.w3schools.com/java/java_interface.asp

[^5]: https://drbtaneja.com/multiple-inheritance-using-interface/

[^6]: https://www.baeldung.com/java-implements-vs-extends

[^7]: https://blog.idrsolutions.com/java-8-default-methods-explained-5-minutes/

[^8]: https://www.baeldung.com/java-interface-private-methods

[^9]: https://javadevcentral.com/default-method-resolution-rules/

[^10]: https://www.geeksforgeeks.org/java/resolving-conflicts-during-multiple-inheritance-in-java/

[^11]: https://www.javabrahman.com/java-8/java-8-multiple-inheritance-conflict-resolution-rules-and-diamond-problem/

[^12]: https://dev.java/learn/implementing-an-interface/

[^13]: https://stackoverflow.com/questions/35962451/what-kind-of-relationship-does-an-interface-have-with-it-implementing-class

[^14]: https://www.reddit.com/r/javahelp/comments/kyjrfe/if_a_class_implements_an_interface_does_this/

[^15]: https://www.ibm.com/docs/en/rsas/7.5.0?topic=elements-creating-implements-relationships-between-java-classes-interfaces

[^16]: https://techvidvan.com/tutorials/implements-in-java/

[^17]: https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/

[^18]: https://www.tutorialspoint.com/java/implements_keyword_in_java.htm

[^19]: https://www.linkedin.com/pulse/default-static-private-methods-java-interfaces-incus-data-pty-ltd-sjhgf

[^20]: https://ioflood.com/blog/implements-java/

[^21]: https://stackoverflow.com/questions/32471220/super-class-method-and-interface-default-method-conflict-resolution

[^22]: https://www.javacodegeeks.com/why-calling-super-super-method-is-not-allowed-in-java.html

[^23]: https://stackoverflow.com/questions/23256406/conflicting-methods-on-interface-multiple-inheritance

[^24]: https://stackoverflow.com/questions/10839131/implements-vs-extends-when-to-use-whats-the-difference

#java #oop #interfaces
