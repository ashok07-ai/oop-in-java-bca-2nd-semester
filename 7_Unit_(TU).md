# Unit 7: Collections and Generics
 
## Table of Contents
 
1. [Wrapper Class and Associated Methods](#71-wrapper-class-and-associated-methods)
2. [Java Collection Framework](#72-java-collection-framework)
3. [List, Set, Map Interface](#73-list-set-map-interface)
4. [Accessing Collections: Iterator/Comparator](#74-accessing-collections-iteratorcomparator)
5. [Defining Generic Class and Methods](#75-defining-generic-class-and-methods)
6. [Generic Interface and Generic Hierarchy](#76-generic-interface-and-generic-hierarchy)
7. [Some Generic Restrictions](#77-some-generic-restrictions)
---
 
## 7.1 Wrapper Class and Associated Methods
 
**Definition:** A **wrapper class** wraps (encloses) a primitive data type into an **object**. Java provides one wrapper class for each primitive type, all present in the `java.lang` package. Wrapper classes are essential because **Collections** (like `ArrayList`) can only store objects, not primitives.
 
### Primitive Type ↔ Wrapper Class Mapping
 
| Primitive | Wrapper Class |
| --------- | -------------- |
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |
 
### Autoboxing and Unboxing
 
| Term | Meaning |
| ---- | ------- |
| **Autoboxing** | Automatic conversion of a primitive into its corresponding wrapper object |
| **Unboxing** | Automatic conversion of a wrapper object back into its primitive value |
 
```java
public class AutoboxingDemo {
    public static void main(String[] args) {
        int a = 10;
        Integer wrappedA = a;          // autoboxing: int -> Integer
        int unwrappedA = wrappedA;     // unboxing: Integer -> int
 
        System.out.println(wrappedA + " " + unwrappedA);
    }
}
```
 
### Common Wrapper Class Methods
 
| Method | Purpose |
| ------ | ------- |
| `parseInt(String)` / `parseDouble(String)` | Converts a `String` to the corresponding primitive |
| `valueOf(String)` | Returns a wrapper object for the given value |
| `toString()` | Converts the wrapper value to a `String` |
| `intValue()`, `doubleValue()`, etc. | Returns the value as the specified primitive type |
| `compareTo()` | Compares two wrapper objects numerically |
| `MAX_VALUE` / `MIN_VALUE` | Constants representing the maximum/minimum value of the type |
 
### Example
 
```java
public class WrapperMethodsDemo {
    public static void main(String[] args) {
        String numStr = "125";
        int num = Integer.parseInt(numStr);       // String -> int
        System.out.println("Parsed value: " + (num + 5));
 
        Integer obj = Integer.valueOf(50);
        System.out.println("Max int value: " + Integer.MAX_VALUE);
        System.out.println("Min int value: " + Integer.MIN_VALUE);
 
        Double d = 12.75;
        System.out.println("As int: " + d.intValue());   // truncates decimal part
    }
}
```
 
---
 
## 7.2 Java Collection Framework
 
**Definition:** The **Collection Framework** is a unified architecture (set of interfaces and classes in `java.util`) for storing and manipulating groups of objects, providing ready-made data structures and algorithms (searching, sorting) so programmers don't need to implement them from scratch.
 
### Framework Hierarchy (Overview)
 
```
                         Iterable
                            │
                        Collection
             ┌──────────────┼──────────────┐
           List            Set            Queue
      (ArrayList,      (HashSet,       (LinkedList,
       LinkedList,      LinkedHashSet,   PriorityQueue)
       Vector)          TreeSet)
 
                          Map  (separate hierarchy - not a Collection)
                 (HashMap, LinkedHashMap, TreeMap)
```
 
### Advantages of Collection Framework
 
- Provides **reusable** data structures (no need to write custom ones).
- Increases performance through optimized algorithms.
- Provides **interoperability** between different collection types.
- Supports **generics**, giving compile-time type safety.
### Core Interfaces
 
| Interface | Description |
| --------- | ----------- |
| `Collection` | Root interface for most collection types (except `Map`) |
| `List` | Ordered collection that allows duplicate elements |
| `Set` | Collection that does **not** allow duplicate elements |
| `Queue` | Collection designed for holding elements prior to processing (FIFO order) |
| `Map` | Stores data as key-value pairs (keys are unique) |
 
---
 
## 7.3 List, Set, Map Interface
 
### 7.3.1 `List` Interface
 
An **ordered** collection (also called a sequence) that allows **duplicate** elements, and elements can be accessed by their **index**.
 
| Implementing Class | Characteristics |
| -------------------- | ---------------- |
| `ArrayList` | Resizable array; fast random access; slower insert/delete in the middle |
| `LinkedList` | Doubly-linked list; fast insert/delete; slower random access |
| `Vector` | Synchronized (thread-safe) resizable array; legacy class |
 
```java
import java.util.ArrayList;
import java.util.List;
 
public class ListDemo {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Apple");            // duplicates allowed
        fruits.add(1, "Mango");         // insert at index 1
 
        System.out.println(fruits);              // [Apple, Mango, Banana, Apple]
        System.out.println(fruits.get(0));        // Apple
        fruits.remove("Banana");
        System.out.println(fruits);
    }
}
```
 
### 7.3.2 `Set` Interface
 
A collection that does **not allow duplicate** elements; models the mathematical set abstraction.
 
| Implementing Class | Characteristics |
| --------------------- | ---------------- |
| `HashSet` | No guaranteed order; fastest performance |
| `LinkedHashSet` | Maintains insertion order |
| `TreeSet` | Maintains elements in **sorted** order (natural ordering or a `Comparator`) |
 
```java
import java.util.Set;
import java.util.HashSet;
import java.util.TreeSet;
 
public class SetDemo {
    public static void main(String[] args) {
        Set<String> names = new HashSet<>();
        names.add("Bishal");
        names.add("Manisha");
        names.add("Bishal");             // duplicate - ignored
 
        System.out.println(names);       // order not guaranteed
 
        Set<Integer> sortedNumbers = new TreeSet<>();
        sortedNumbers.add(50);
        sortedNumbers.add(10);
        sortedNumbers.add(30);
        System.out.println(sortedNumbers);   // [10, 30, 50] - sorted automatically
    }
}
```
 
### 7.3.3 `Map` Interface
 
Stores data as **key-value pairs**; keys must be **unique**, but values can be duplicated. `Map` does **not** extend the `Collection` interface — it is a separate hierarchy.
 
| Implementing Class | Characteristics |
| --------------------- | ---------------- |
| `HashMap` | No guaranteed order; allows one `null` key |
| `LinkedHashMap` | Maintains insertion order |
| `TreeMap` | Maintains keys in sorted order |
 
```java
import java.util.Map;
import java.util.HashMap;
 
public class MapDemo {
    public static void main(String[] args) {
        Map<Integer, String> students = new HashMap<>();
        students.put(101, "Alpha alpha");
        students.put(102, "Beta beta");
        students.put(101, "Gamma gamma");     // overwrites value for key 101
 
        System.out.println(students.get(101));         // Gamma gamma
        System.out.println(students.containsKey(102)); // true
 
        for (Map.Entry<Integer, String> entry : students.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```
 
### Comparison Summary
 
| Interface | Duplicates | Order | Access by |
| --------- | :--------: | ----- | --------- |
| `List` | Allowed | Insertion order (index-based) | Index |
| `Set` | Not allowed | Depends on implementation | Iteration only |
| `Map` | Keys unique, values can repeat | Depends on implementation | Key |
 
---
 
## 7.4 Accessing Collections: Iterator/Comparator
 
### 7.4.1 `Iterator`
 
**Definition:** `Iterator` is an interface used to **traverse** (loop through) a collection one element at a time and optionally **remove** elements during traversal.
 
| Method | Purpose |
| ------ | ------- |
| `hasNext()` | Returns `true` if more elements exist |
| `next()` | Returns the next element in the collection |
| `remove()` | Removes the last element returned by `next()` |
 
```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
 
public class IteratorDemo {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(List.of(10, 15, 20, 25, 30));
 
        Iterator<Integer> it = numbers.iterator();
        while (it.hasNext()) {
            int value = it.next();
            if (value % 2 == 0) {
                it.remove();          // safely removes elements while iterating
            }
        }
        System.out.println(numbers);   // [15, 25]
    }
}
```
 
**Note:** Removing elements directly from a `List` (e.g., `numbers.remove()`) while using a `for-each` loop causes a `ConcurrentModificationException`; `Iterator.remove()` is the safe way to do it.
 
### 7.4.2 `Comparator`
 
**Definition:** `Comparator` is a functional interface used to define a **custom ordering** for objects, independent of their natural ordering (`Comparable`). Useful when sorting by multiple/different criteria.
 
| Basis | `Comparable` | `Comparator` |
| ----- | ------------- | ------------- |
| Package | `java.lang` | `java.util` |
| Method | `compareTo(Object o)` | `compare(Object o1, Object o2)` |
| Sorting logic location | Inside the class itself | In a separate class (or lambda) |
| Number of sort sequences | Only one (natural order) | Multiple possible |
 
```java
import java.util.*;
 
class Student {
    String name;
    int marks;
    Student(String name, int marks) { this.name = name; this.marks = marks; }
    public String toString() { return name + ":" + marks; }
}
 
public class ComparatorDemo {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>();
        students.add(new Student("Manisha", 78));
        students.add(new Student("Bishal", 92));
        students.add(new Student("Alpha", 65));
 
        // Sorting using a Comparator (descending order of marks)
        students.sort(new Comparator<Student>() {
            public int compare(Student s1, Student s2) {
                return s2.marks - s1.marks;
            }
        });
        System.out.println(students);
 
        // Same sort using a lambda expression (shorter syntax)
        students.sort((s1, s2) -> s1.name.compareTo(s2.name));   // ascending by name
        System.out.println(students);
    }
}
```
 
---
 
## 7.5 Defining Generic Class and Methods
 
**Definition:** **Generics** allow classes, interfaces, and methods to operate on a **type parameter** specified at the time of use, providing **compile-time type safety** and eliminating the need for explicit typecasting. Generic type parameters are conventionally written as single uppercase letters (`T`, `E`, `K`, `V`).
 
### Generic Class
 
```java
class Box<T> {                       // T is a type parameter
    private T item;
 
    void setItem(T item) { this.item = item; }
    T getItem() { return item; }
}
 
public class GenericClassDemo {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.setItem("Hello Generics");
        System.out.println(stringBox.getItem());
 
        Box<Integer> intBox = new Box<>();
        intBox.setItem(100);
        System.out.println(intBox.getItem());
        // no casting needed, and compiler prevents type mismatch at compile time
    }
}
```
 
### Generic Method
 
A method can declare its own type parameter, independent of whether its class is generic.
 
```java
public class GenericMethodDemo {
    // generic method - <T> declared before the return type
    static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }
 
    public static void main(String[] args) {
        Integer[] intArray = {1, 2, 3, 4};
        String[] strArray = {"A", "B", "C"};
 
        printArray(intArray);
        printArray(strArray);
    }
}
```
 
### Multiple Type Parameters
 
```java
class Pair<K, V> {
    K key;
    V value;
    Pair(K key, V value) { this.key = key; this.value = value; }
    public String toString() { return key + " = " + value; }
}
 
public class MultiTypeParamDemo {
    public static void main(String[] args) {
        Pair<String, Integer> studentAge = new Pair<>("Bishal", 21);
        System.out.println(studentAge);
    }
}
```
 
---
 
## 7.6 Generic Interface and Generic Hierarchy
 
### Generic Interface
 
An interface can also declare type parameters, which must be provided (or further parameterized) by implementing classes.
 
```java
interface Container<T> {
    void add(T item);
    T get();
}
 
class SimpleContainer<T> implements Container<T> {
    private T item;
    public void add(T item) { this.item = item; }
    public T get() { return item; }
}
 
public class GenericInterfaceDemo {
    public static void main(String[] args) {
        Container<String> container = new SimpleContainer<>();
        container.add("Generic Interface Example");
        System.out.println(container.get());
    }
}
```
 
### Generic Hierarchy (Bounded Types and Inheritance)
 
A generic class can be extended, and **bounded type parameters** restrict what types can be substituted using the `extends` keyword.
 
```java
// Bounded type parameter: T must be a Number or a subclass of Number
class NumericBox<T extends Number> {
    T value;
    NumericBox(T value) { this.value = value; }
 
    double doubleValue() {
        return value.doubleValue();     // safe - guaranteed to be a Number
    }
}
 
// Generic class inheritance
class Box<T> {
    T content;
    void set(T content) { this.content = content; }
}
 
class SpecialBox<T> extends Box<T> {    // subclass keeps the type parameter generic
    void display() {
        System.out.println("Content: " + content);
    }
}
 
public class GenericHierarchyDemo {
    public static void main(String[] args) {
        NumericBox<Integer> nBox = new NumericBox<>(42);
        System.out.println(nBox.doubleValue());
 
        SpecialBox<String> sBox = new SpecialBox<>();
        sBox.set("Inherited Generic Box");
        sBox.display();
    }
}
```
 
### Wildcards (`?`)
 
| Wildcard | Meaning |
| -------- | ------- |
| `<?>` | Unbounded wildcard - accepts any type |
| `<? extends T>` | Upper bounded - accepts `T` or any subclass of `T` |
| `<? super T>` | Lower bounded - accepts `T` or any superclass of `T` |
 
```java
static void printList(List<?> list) {     // accepts a List of any type
    for (Object o : list) {
        System.out.println(o);
    }
}
```
 
---
 
## 7.7 Some Generic Restrictions
 
Java generics are implemented via **type erasure** (generic type information exists only at compile time and is removed at runtime), which leads to several restrictions:
 
| Restriction | Explanation |
| ----------- | ----------- |
| **Cannot instantiate generic type with primitives** | `Box<int>` is invalid; must use wrapper classes, e.g., `Box<Integer>` |
| **Cannot create instances of type parameters** | `new T()` is not allowed, since the actual type is unknown at compile time |
| **Cannot create arrays of parameterized types** | `new T[10]` or `new List<String>[5]` is not allowed |
| **Cannot use `instanceof` with parameterized types** | `if (obj instanceof T)` and `if (list instanceof List<String>)` are invalid due to type erasure |
| **Cannot create static fields of type parameters** | A `static T field;` is not allowed since type parameters belong to instances, not the class itself |
| **Cannot throw or catch instances of a generic class** | A generic class cannot extend `Throwable` |
| **Cannot overload a method where erasure produces the same signature** | `void test(List<String> l)` and `void test(List<Integer> l)` cannot coexist - both erase to `void test(List l)` |
 
### Example (Illustrating Restrictions)
 
```java
class Demo<T> {
    // T item = new T();            // ERROR - cannot instantiate type parameter
    // T[] array = new T[10];       // ERROR - cannot create generic array
    // static T value;              // ERROR - cannot use type parameter in static context
 
    T item;
    Demo(T item) { this.item = item; }
 
    boolean checkType(Object obj) {
        // return obj instanceof T;  // ERROR - illegal generic type check
        return obj != null;
    }
}
```
 
---
 