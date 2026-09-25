# Unit 8: Advanced OOP Concepts in Java
 
## Table of Contents
 
1. [Design Patterns: Singleton, Factory, Observer Pattern](#81-design-patterns-singleton-factory-observer-pattern)
2. [Lambda Expression](#82-lambda-expression)
3. [Stream API: Introduction](#83-stream-api-introduction)
4. [Optional Class](#84-optional-class)
5. [Method References](#85-method-references)
---
 
## 8.1 Design Patterns: Singleton, Factory, Observer Pattern
 
**Definition:** A **design pattern** is a general, reusable solution to a commonly occurring problem in software design. Patterns are not finished code but templates/best-practice structures that can be adapted to a specific situation.
 
### 8.1.1 Singleton Pattern
 
**Definition:** Ensures a class has **only one instance** throughout the application and provides a single **global point of access** to it. Commonly used for logging, configuration managers, database connections, etc.
 
```java
class Singleton {

    // 1. Create a private static object
    private static Singleton instance;

    // 2. Make constructor private
    private Singleton() {
        System.out.println("Singleton object created");
    }

    // 3. Provide a public method to get the object
    public static Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }
}

public class SingleTonPatternExapmle {

    public static void main(String[] args) {

        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();

        obj1.showMessage();

        // Check whether both objects are the same
        System.out.println(obj1 == obj2);
    }
}
```
 
### 8.1.2 Factory Pattern
 
**Definition:** Defines a separate method/class for **creating objects**, letting subclasses (or a factory method) decide which concrete class to instantiate — hiding object-creation logic from the client code.
 
```java
// Product interface
interface Vehicle {
    void drive();
}

// Concrete Product 1
class Car implements Vehicle {
    public void drive() {
        System.out.println("Driving a car");
    }
}

// Concrete Product 2
class Bike implements Vehicle {
    public void drive() {
        System.out.println("Riding a bike");
    }
}

// Factory
class VehicleFactory {

    public Vehicle getVehicle(String type) {

        if (type.equalsIgnoreCase("car")) {
            return new Car();
        }
        else if (type.equalsIgnoreCase("bike")) {
            return new Bike();
        }

        return null;
    }
}

// Main class
public class FactoryPatternExample {

    public static void main(String[] args) {

        VehicleFactory factory = new VehicleFactory();

        Vehicle v1 = factory.getVehicle("car");
        v1.drive();

        Vehicle v2 = factory.getVehicle("bike");
        v2.drive();
    }
}
```
 
### 8.1.3 Observer Pattern
 
**Definition:** Defines a **one-to-many dependency** between objects so that when one object (the **subject**) changes state, all its dependents (**observers**) are automatically notified and updated. Commonly used in event-handling systems, GUIs, and notification services in youtube or group chats.
 
```java
import java.util.ArrayList;
import java.util.List;

// Observer
interface Subscriber {
    void update(String video);
}

// Concrete Observer
class User implements Subscriber {

    private String name;

    User(String name) {
        this.name = name;
    }

    public void update(String video) {
        System.out.println(name + " received notification: " + video);
    }
}

// Subject
class YouTubeChannel {

    private List<Subscriber> subscribers = new ArrayList<>();

    // Add subscriber
    public void subscribe(Subscriber subscriber) {
        subscribers.add(subscriber);
    }

    // Upload video
    public void uploadVideo(String video) {
        System.out.println("\nNew video uploaded: " + video);

        // Notify all subscribers
        for (Subscriber subscriber : subscribers) {
            subscriber.update(video);
        }
    }
}

// Main class
public class ObserverPatternExmaple {

    public static void main(String[] args) {

        YouTubeChannel channel = new YouTubeChannel();

        Subscriber user1 = new User("Alpha");
        Subscriber user2 = new User("Beta");

        channel.subscribe(user1);
        channel.subscribe(user2);

        channel.uploadVideo("Java Observer Pattern");
    }
}
```
 
### Pattern Comparison
 
| Pattern | Category | Purpose |
| ------- | -------- | ------- |
| **Singleton** | Creational | Guarantees a single, shared instance of a class |
| **Factory** | Creational | Centralizes and hides object creation logic |
| **Observer** | Behavioral | Notifies dependent objects automatically of state changes |
 
---
 
## 8.2 Lambda Expression
 
**Definition:** A **lambda expression** (introduced in Java 8) provides a clear and concise way to represent an instance of a **functional interface** (an interface with exactly one abstract method) using an expression, reducing boilerplate code compared to anonymous inner classes.
 
### Syntax
 
```
(parameters) -> { body }
```
 
| Part | Description |
| ---- | ----------- |
| `(parameters)` | Input parameters (types are usually inferred) |
| `->` | Lambda (arrow) operator, separates parameters from body |
| `{ body }` | The implementation logic; braces optional for a single statement |
 
### Example: Before and After Lambda
 
```java
interface Message {
  void show();
}

public class Example {
  public static void main(String[] args) {
    // Without Lambda expression
    Message m1 = new Message(){
      public void show(){
        System.out.println("Hello message");
      }
    };
    m1.show();

  // With Lambda Expression
    Message m = () -> {
      System.out.println("Hello message");
    };
    m.show();
  }
}
```
 
### Lambda with Collections (Sorting Example)
 
```java
import java.util.*;
 
public class LambdaSortDemo {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>(List.of("Manisha", "Alpha", "Bishal"));
        names.sort((a, b) -> a.compareTo(b));    // ascending order using lambda
        System.out.println(names);
 
        names.forEach(name -> System.out.println("Name: " + name));  // lambda with forEach
    }
}
```
 
---
 
## 8.3 Stream API: Introduction
 
**Definition:** The **Stream API** (introduced in Java 8, `java.util.stream` package) allows processing sequences of elements (from collections, arrays, etc.) in a **declarative, functional style** — supporting operations like filter, map, and reduce, and enabling easy parallel processing. A stream does **not** store data; it operates on the data source and produces a result.
 
### Stream Operation Types
 
| Type | Description | Examples |
| ---- | ----------- | -------- |
| **Intermediate** | Returns another stream, allowing chaining; **lazy** (not executed until a terminal operation is called) | `filter()`, `map()`, `sorted()`, `distinct()`, `limit()` |
| **Terminal** | Produces a final result and closes the stream | `forEach()`, `collect()`, `reduce()`, `count()`, `sum()` |
 
### Example: Basic Stream Pipeline
 
```java
import java.util.ArrayList;

public class Example2 {
  public static void main(String[] args) {
    ArrayList<Integer> numbers = new ArrayList<>();
    numbers.add(10);
    numbers.add(20);
    numbers.add(40);
    numbers.add(5);
    numbers.add(100);
    numbers.add(30);
    numbers.add(4);

    // filter numbers greater than 20 -> collect them -> and back to list
    numbers.stream()
    .filter(n -> n > 20)
    .forEach(n -> {
      System.out.println("Filtered numbers: " + n);
    });

    
  }
}

```
 
### Common Stream Methods
 
| Method | Purpose |
| ------ | ------- |
| `filter(Predicate)` | Keeps only elements that satisfy a condition |
| `map(Function)` | Transforms each element |
| `sorted()` | Sorts elements (natural or via `Comparator`) |
| `distinct()` | Removes duplicate elements |
| `limit(n)` | Restricts the stream to the first `n` elements |
| `collect()` | Gathers stream results into a collection (List, Set, Map) |
| `reduce()` | Combines elements into a single result |
| `count()` | Returns the number of elements |
| `forEach()` | Performs an action on each element |
 
### Example: `reduce()` and Working with Strings
 
```java
import java.util.*;
import java.util.stream.*;
 
public class StreamReduceDemo {
    public static void main(String[] args) {
        List<Integer> marks = List.of(70, 85, 90, 60, 95);
 
        int total = marks.stream()
                .reduce(0, (a, b) -> a + b);       // reduce - sums all elements
        System.out.println("Total: " + total);
 
        double average = marks.stream()
                .mapToInt(Integer::intValue)
                .average()
                .orElse(0);
        System.out.println("Average: " + average);
 
        List<String> names = List.of("Manisha", "Bishal", "Alpha", "Bishal");
        List<String> uniqueSorted = names.stream()
                .distinct()
                .sorted()
                .collect(Collectors.toList());
        System.out.println(uniqueSorted);
    }
}
```
 
---
 
## 8.4 Optional Class
 
**Definition:** `Optional<T>` (introduced in Java 8, `java.util` package) is a **container object** which may or may not contain a non-null value. It is used to represent the possible absence of a value explicitly, helping to avoid `NullPointerException` and eliminating the need for excessive null checks.
 
### Creating an `Optional`
 
| Method | Purpose |
| ------ | ------- |
| `Optional.of(value)` | Creates an `Optional` with a non-null value (throws exception if `null`) |
| `Optional.ofNullable(value)` | Creates an `Optional` that may hold `null` safely |
| `Optional.empty()` | Creates an empty `Optional` |
 
### Common Methods
 
| Method | Purpose |
| ------ | ------- |
| `isPresent()` | Returns `true` if a value is present |
| `isEmpty()` | Returns `true` if no value is present (Java 11+) |
| `get()` | Returns the value (throws exception if empty - use with caution) |
| `orElse(default)` | Returns the value, or a default if empty |
| `orElseGet(Supplier)` | Returns the value, or computes a default lazily if empty |
| `orElseThrow()` | Returns the value, or throws an exception if empty |
| `ifPresent(Consumer)` | Executes an action only if a value is present |
 
### Example_1

```java
import java.util.Optional;

public class OptionalClassExample {
  public static void main(String[] args) {
    String name = null;
    Optional<String> result = Optional.ofNullable(name);
    System.out.println(result);
  }
}
```

### Example_2
```java
import java.util.Optional;
 
public class OptionalDemo {
    static Optional<String> findUserById(int id) {
        if (id == 1) {
            return Optional.of("Manisha KC");
        }
        return Optional.empty();          // simulate "not found"
    }
 
    public static void main(String[] args) {
        Optional<String> user1 = findUserById(1);
        Optional<String> user2 = findUserById(2);
 
        // Safe access - no NullPointerException risk
        System.out.println(user1.orElse("User not found"));   // Manisha KC
        System.out.println(user2.orElse("User not found"));   // User not found
 
        user1.ifPresent(name -> System.out.println("Found user: " + name));
 
        String result = user2.orElseGet(() -> "Default Guest User");
        System.out.println(result);
 
        try {
            user2.orElseThrow(() -> new RuntimeException("User does not exist!"));
        } catch (RuntimeException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}
```
 
---
 
## 8.5 Method References
 
**Definition:** A **method reference** is a shorthand notation for a lambda expression that simply calls an **existing method**. It uses the `::` (double colon) operator and improves readability when a lambda would do nothing more than delegate to another method.
 
### Types of Method References
 
| Type | Syntax | Example |
| ---- | ------ | ------- |
| Reference to a **static method** | `ClassName::staticMethod` | `Math::sqrt` |
| Reference to an **instance method** of a particular object | `object::instanceMethod` | `System.out::println` |
| Reference to an **instance method** of an arbitrary object of a particular type | `ClassName::instanceMethod` | `String::toUpperCase` |
| Reference to a **constructor** | `ClassName::new` | `ArrayList::new` |
 
### Example: Static Method Reference
 
```java
import java.util.function.Function;
 
public class StaticMethodRefDemo {
    public static void main(String[] args) {
        // Lambda version
        Function<String, Integer> parseLambda = s -> Integer.parseInt(s);
        // Method reference version - equivalent and more concise
        Function<String, Integer> parseRef = Integer::parseInt;
 
        System.out.println(parseRef.apply("123"));
    }
}
```
 
### Example: Instance Method of a Particular Object
 
```java
import java.util.function.Consumer;
 
public class InstanceMethodRefDemo {
    public static void main(String[] args) {
        // Lambda version
        Consumer<String> printLambda = s -> System.out.println(s);
        // Method reference version
        Consumer<String> printRef = System.out::println;
 
        printRef.accept("Using an instance method reference");
    }
}
```
 
### Example: Instance Method of an Arbitrary Object of a Particular Type
 
```java
import java.util.*;
import java.util.stream.*;
 
public class ArbitraryObjectMethodRefDemo {
    public static void main(String[] args) {
        List<String> names = List.of("manisha", "bishal", "alpha");
 
        // Lambda version
        names.stream().map(s -> s.toUpperCase()).forEach(System.out::println);
 
        // Method reference version - equivalent
        List<String> upperNames = names.stream()
                .map(String::toUpperCase)          // arbitrary object of type String
                .collect(Collectors.toList());
        System.out.println(upperNames);
    }
}
```
 
### Example: Constructor Reference
 
```java
import java.util.function.Supplier;
import java.util.ArrayList;
import java.util.List;
 
public class ConstructorRefDemo {
    public static void main(String[] args) {
        // Lambda version
        Supplier<List<String>> listSupplierLambda = () -> new ArrayList<>();
        // Method reference version
        Supplier<List<String>> listSupplierRef = ArrayList::new;
 
        List<String> newList = listSupplierRef.get();
        newList.add("Created using a constructor reference");
        System.out.println(newList);
    }
}
```
 
---
