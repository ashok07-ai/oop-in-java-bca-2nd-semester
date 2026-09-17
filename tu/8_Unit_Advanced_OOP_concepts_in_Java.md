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
class DatabaseConnection {
    // single static instance, created only once
    private static DatabaseConnection instance;
 
    private DatabaseConnection() {       // private constructor prevents outside instantiation
        System.out.println("Database connection created");
    }
 
    public static DatabaseConnection getInstance() {
        if (instance == null) {                     // lazy initialization
            instance = new DatabaseConnection();
        }
        return instance;
    }
 
    void query(String sql) {
        System.out.println("Executing: " + sql);
    }
}
 
public class SingletonDemo {
    public static void main(String[] args) {
        DatabaseConnection db1 = DatabaseConnection.getInstance();
        DatabaseConnection db2 = DatabaseConnection.getInstance();
 
        db1.query("SELECT * FROM students");
        System.out.println("Same instance? " + (db1 == db2));   // true
    }
}
```
 
### 8.1.2 Factory Pattern
 
**Definition:** Defines a separate method/class for **creating objects**, letting subclasses (or a factory method) decide which concrete class to instantiate — hiding object-creation logic from the client code.
 
```java
interface Shape {
    void draw();
}
 
class Circle implements Shape {
    public void draw() { System.out.println("Drawing a Circle"); }
}
 
class Square implements Shape {
    public void draw() { System.out.println("Drawing a Square"); }
}
 
// Factory class - decides which object to create
class ShapeFactory {
    static Shape getShape(String type) {
        switch (type.toLowerCase()) {
            case "circle": return new Circle();
            case "square": return new Square();
            default: throw new IllegalArgumentException("Unknown shape: " + type);
        }
    }
}
 
public class FactoryDemo {
    public static void main(String[] args) {
        Shape s1 = ShapeFactory.getShape("circle");
        s1.draw();
 
        Shape s2 = ShapeFactory.getShape("square");
        s2.draw();
        // Client code does not need to know Circle/Square class details
    }
}
```
 
### 8.1.3 Observer Pattern
 
**Definition:** Defines a **one-to-many dependency** between objects so that when one object (the **subject**) changes state, all its dependents (**observers**) are automatically notified and updated. Commonly used in event-handling systems, GUIs, and notification services.
 
```java
import java.util.ArrayList;
import java.util.List;
 
// Observer interface
interface Subscriber {
    void update(String news);
}
 
// Subject - maintains a list of observers
class NewsChannel {
    private List<Subscriber> subscribers = new ArrayList<>();
 
    void subscribe(Subscriber s) { subscribers.add(s); }
 
    void publishNews(String news) {
        System.out.println("Publishing: " + news);
        for (Subscriber s : subscribers) {
            s.update(news);         // notify all observers
        }
    }
}
 
// Concrete observer
class Viewer implements Subscriber {
    String name;
    Viewer(String name) { this.name = name; }
    public void update(String news) {
        System.out.println(name + " received update: " + news);
    }
}
 
public class ObserverDemo {
    public static void main(String[] args) {
        NewsChannel channel = new NewsChannel();
        channel.subscribe(new Viewer("Manisha"));
        channel.subscribe(new Viewer("Bishal"));
 
        channel.publishNews("Breaking News: Java 21 released!");
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
interface Greeting {
    void greet(String name);
}
 
public class LambdaDemo {
    public static void main(String[] args) {
        // Before Java 8 - anonymous inner class
        Greeting g1 = new Greeting() {
            public void greet(String name) {
                System.out.println("Hello (anonymous), " + name);
            }
        };
        g1.greet("Alpha");
 
        // Using a lambda expression - much shorter
        Greeting g2 = (name) -> System.out.println("Hello (lambda), " + name);
        g2.greet("Beta");
    }
}
```
 
### Lambdas with Built-in Functional Interfaces
 
```java
import java.util.function.*;
 
public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Predicate<T> - takes T, returns boolean
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println(isEven.test(10));      // true
 
        // Function<T,R> - takes T, returns R
        Function<Integer, Integer> square = n -> n * n;
        System.out.println(square.apply(5));       // 25
 
        // Consumer<T> - takes T, returns nothing
        Consumer<String> printer = s -> System.out.println("Value: " + s);
        printer.accept("Lambda expressions");
 
        // Supplier<T> - takes nothing, returns T
        Supplier<String> supplier = () -> "Generated value";
        System.out.println(supplier.get());
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
import java.util.*;
import java.util.stream.*;
 
public class StreamDemo {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(4, 9, 15, 20, 25, 30, 35);
 
        // filter -> even numbers, map -> square them, collect -> back to a List
        List<Integer> result = numbers.stream()
                .filter(n -> n % 2 == 0)       // intermediate operation
                .map(n -> n * n)                // intermediate operation
                .collect(Collectors.toList());  // terminal operation
 
        System.out.println(result);              // [400, 900]
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
 
### Example
 
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