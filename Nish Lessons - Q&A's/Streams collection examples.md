<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# give me one example of a stream being used for each different collection type including an example for objects

Here are concise, runnable stream examples for List, Set, Map, and Queue/Deque, plus an example using custom objects, all idiomatic for modern Java 17+. Streams are obtained from collections via the `Collection.stream()` default method, and many pipelines end with `toList()` (immutable in Java 16+) or collectors for specific targets.[^1][^2][^3]

## List example

- Lists expose `stream()` to build pipelines like filter/map and then finish with a terminal operation such as `toList()` to produce results.[^2][^1]

```java
// List -> filter + map -> List (immutable result in Java 16+)
import java.util.*;

public class ListStreamDemo {
    public static void main(String[] args) {
        List<String> names = List.of("John", "Paul", "George", "Ringo");
        List<String> jUpper = names.stream()                 // source from Collection.stream() [concept]
            .filter(n -> n.startsWith("J"))                  // intermediate: lazy
            .map(String::toUpperCase)                        // intermediate: lazy
            .toList();                                       // terminal: executes pipeline, immutable List
        System.out.println(jUpper);
    }
}
```

- `toList()` is a terminal method on `Stream` that creates an unmodifiable list, whereas `Collectors.toList()` typically returns a mutable implementation.[^3][^2]


## Set example

- Sets also inherit `stream()` from `Collection`, and results can be collected using collectors like `Collectors.toSet()` for a set target.[^4][^1]

```java
// Set -> filter -> Set
import java.util.*;
import java.util.stream.Collectors;

public class SetStreamDemo {
    public static void main(String[] args) {
        Set<Integer> nums = Set.of(1, 2, 3, 4, 5, 6);
        Set<Integer> evens = nums.stream()
            .filter(n -> n % 2 == 0)
            .collect(Collectors.toSet()); // mutable Set
        System.out.println(evens);
    }
}
```

- Collectors live in `java.util.stream` and provide common mutable reductions like `toSet`, `toMap`, and `groupingBy` for stream results.[^4]


## Map example

- Maps are not `Collection`s, but streaming is idiomatic via views like `entrySet()` or `values()`, e.g., `map.entrySet().stream()` with `Collectors.toMap(...)` to rebuild filtered maps.[^5][^4]

```java
// Map<K,V> -> filter entries -> Map<K,V>
import java.util.*;
import java.util.stream.Collectors;

public class MapStreamDemo {
    public static void main(String[] args) {
        Map<String, Integer> ages = Map.of("Ada", 36, "Babbage", 45, "Turing", 41);
        Map<String, Integer> over40 = ages.entrySet().stream()   // stream entries
            .filter(e -> e.getValue() > 40)
            .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
        System.out.println(over40);
    }
}
```

- Oracle’s `Map.Entry` docs even show an API note with `map.entrySet().stream().map(Map.Entry::copyOf).toList()` to take a snapshot, reinforcing the entry-stream pattern.[^5]


## Queue/Deque example

- Queue/Deque types like `ArrayDeque` or `LinkedList` implement `Collection`, so they inherit `stream()`, and a common pattern is to collect into a `Deque` using `Collectors.toCollection(ArrayDeque::new)`.[^6][^1]

```java
// Deque -> filter -> Deque
import java.util.*;
import java.util.stream.Collectors;

public class DequeStreamDemo {
    public static void main(String[] args) {
        Deque<Integer> dq = new ArrayDeque<>(List.of(5, 2, 7, 2, 9));
        Deque<Integer> twos = dq.stream()
            .filter(n -> n == 2)
            .collect(Collectors.toCollection(ArrayDeque::new)); // collect into Deque
        System.out.println(twos);
    }
}
```

- Because `Deque` is a `Collection`, the same pipeline patterns apply, and specialized collectors help target specific concrete types when needed.[^1][^6]


## Objects example (custom type)

- Streams shine with domain objects by composing readable filters, mappings, and reductions on `List<Person>` or similar sources.[^2][^4]

```java
// Filter objects, map fields, group results
import java.util.*;
import java.util.stream.Collectors;

record Person(String name, int age, String city) {}

public class ObjectStreamDemo {
    public static void main(String[] args) {
        List<Person> people = List.of(
            new Person("Ada", 36, "London"),
            new Person("Babbage", 45, "London"),
            new Person("Turing", 41, "Manchester")
        );

        // Names of Londoners (immutable List)
        List<String> londonerNames = people.stream()
            .filter(p -> p.city().equals("London"))
            .map(Person::name)
            .toList();
        System.out.println(londonerNames);

        // Group by city
        Map<String, List<Person>> byCity = people.stream()
            .collect(Collectors.groupingBy(Person::city));
        System.out.println(byCity);
    }
}
```

- Pipelines use lazy intermediate operations (`filter`, `map`) and run only at a terminal step like `toList()` or `collect(...)`, consuming the stream once.[^2][^4]

> [!TIP]
> All Java collections expose `stream()` via `Collection`, so the same pipeline style applies across Lists, Sets, and Deques/Queues, while Maps require streaming over `entrySet()`/`keySet()`/`values()` views first.[^1][^5]

Tags: \#java \#java/streams \#java/collections \#bestpractices
See also: [[Java Collections]] [[Design Patterns]][^4][^2]
<span style="display:none">[^10][^11][^12][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collection.html

[^2]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/stream/Stream.html

[^3]: https://stackoverflow.com/questions/65969919/differences-of-java-16s-stream-tolist-and-stream-collectcollectors-tolist

[^4]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/stream/package-summary.html

[^5]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Map.Entry.html

[^6]: https://stackoverflow.com/questions/50411630/collect-java8-intstream-into-deque-interface

[^7]: https://www.baeldung.com/java-8-streams

[^8]: https://bell-sw.com/blog/a-guide-to-java-stream-api/

[^9]: https://mkyong.com/java8/java-8-filter-a-map-examples/

[^10]: https://www.geeksforgeeks.org/java/deque-interface-java-example/

[^11]: https://www.baeldung.com/java-17-new-features

[^12]: https://download.java.net/java/early_access/valhalla/docs/api/java.base/java/util/Map.Entry.html

