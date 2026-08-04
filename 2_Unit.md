# Unit 2: Basics of Java Programming
 
## Table of Contents (Unit 2)
1. [Writing Comments and its Types](#21-writing-comments-and-its-types)
2. [Java Tokens: Keywords, Identifiers, Literals, Operators, Separators](#22-java-tokens-keywords-identifiers-literals-operators-separators)
3. [Data Types: Primitive and User-Defined](#23-data-types-primitive-and-user-defined)
4. [Variable Declaration, Assignment, Expression](#24-variable-declaration-and-assignment-expression)
5. [Control Statements](#25-control-statements)
6. [Arrays](#26-arrays)
7. [Type Conversion and Casting](#27-type-conversion-and-casting)
8. [Garbage Collection](#28-garbage-collection)
9. [String Handling](#29-string-creation-concatenation-comparison-modification-changing-case-and-searching)
10. [StringBuffer Class](#210-stringbuffer-class)
---
 
## 2.1 Writing Comments and its Types
 
**Definition:** Comments are non-executable statements in source code used to explain logic, improve readability, and are ignored by the compiler.
 
### Types
 
| Type | Syntax | Use |
|---|---|---|
| **Single-line comment** | `// comment text` | Used for short, one-line explanations |
| **Multi-line comment** | `/* comment text */` | Used for explanations spanning multiple lines |
| **Documentation comment** | `/** comment text */` | Used to generate official documentation using the `javadoc` tool |
 
### Example
 
```java
public class CommentDemo {
    // This is a single-line comment
    /*
       This is a multi-line comment
       explaining the purpose of main method
    */
    /**
     * This is a documentation comment.
     * @author Student
     */
    public static void main(String[] args) {
        System.out.println("Comments Demo"); // prints a message
    }
}
```
 
---
 
## 2.2 Java Token: Keywords, Identifier, Literal, Operators and Separators
 
**Definition:** A **token** is the smallest individual unit of a Java program that the compiler recognizes — such as keywords, identifiers, literals, operators, and separators.
 
### 2.2.1 Keywords
Reserved words with predefined meaning that cannot be used as identifiers. Examples: `class`, `public`, `static`, `void`, `int`, `if`, `else`, `for`, `while`, `return`, `new`, `this`, `super`, `extends`, `implements`, `try`, `catch` (Java has 50+ keywords; `true`, `false`, `null` are literals, not keywords, technically).
 
### 2.2.2 Identifiers
Names given to variables, methods, classes, objects. Rules:
- Can contain letters, digits, `_`, `$`
- Cannot start with a digit
- Cannot be a keyword
- Case-sensitive
Valid: `rollNo`, `_marks`, `$total`, `studentName`
Invalid: `2ndYear` (starts with digit), `class` (keyword)
 
### 2.2.3 Literals
Fixed constant values assigned directly in code.
 
| Literal Type | Example |
|---|---|
| Integer literal | `100`, `0x1F` (hex), `010` (octal) |
| Floating-point literal | `3.14`, `2.5f` |
| Character literal | `'A'`, `'\n'` |
| String literal | `"Kathmandu"` |
| Boolean literal | `true`, `false` |
| Null literal | `null` |
 
### 2.2.4 Operators
 
| Category | Operators | Example |
|---|---|---|
| Arithmetic | `+ - * / %` | `a + b` |
| Relational | `== != > < >= <=` | `a > b` |
| Logical | `&& \|\| !` | `a > 0 && b > 0` |
| Assignment | `= += -= *= /=` | `a += 5` |
| Unary | `++ -- + - !` | `a++` |
| Bitwise | `& \| ^ ~ << >>` | `a & b` |
| Ternary (conditional) | `? :` | `(a>b) ? a : b` |
 
### 2.2.5 Separators (Punctuators)
Symbols used to separate code elements: `( )`, `{ }`, `[ ]`, `;`, `,`, `.`
 
---
 
## 2.3 Data Types: Primitive and User-Defined Data Type
 
**Definition:** A data type specifies the type and size of data a variable can hold.
 
```
                  Java Data Types
                        │
        ┌───────────────┴───────────────┐
   Primitive Types                 Non-Primitive / Reference Types
   (predefined by Java)             (user-defined / derived)
        │                                            │
 ┌───┬───┬────┬────┬─────┬──────┬───────┐        ┌──────┬───────┬─────────┐
byte short int long float double char boolean  Class  Array Interface  String
```
 
### 2.3.1 Primitive Data Types
 
| Type | Size | Default Value | Example |
|---|---|---|---|
| `byte` | 1 byte | 0 | `byte b = 10;` |
| `short` | 2 bytes | 0 | `short s = 200;` |
| `int` | 4 bytes | 0 | `int age = 20;` |
| `long` | 8 bytes | 0L | `long pop = 29000000L;` |
| `float` | 4 bytes | 0.0f | `float pi = 3.14f;` |
| `double` | 8 bytes | 0.0 | `double amt = 4500.75;` |
| `char` | 2 bytes | '\u0000' | `char grade = 'A';` |
| `boolean` | 1 bit (JVM dependent) | false | `boolean pass = true;` |
 
### 2.3.2 User-Defined (Reference/Non-Primitive) Data Types
Created by the programmer or from library classes: **Class, Interface, Array, String**. They store a **reference (address)** to the object, not the value directly.
 
```java
class Student { }               // user-defined type (class)
Student s1 = new Student();     // s1 is a reference variable
int[] marks = new int[5];       // array – user defined/derived type
String name = "Anita";          // String – reference type
```
 
### Difference: Primitive vs Non-Primitive
 
| Basis | Primitive | Non-Primitive |
|---|---|---|
| Defined by | Java language itself | Programmer / Java classes |
| Stores | Actual value | Reference/address of object |
| Size | Fixed | Depends on object |
| Default value | 0 / false / '\u0000' | `null` |
| Example | int, char, boolean | String, Array, Class, Interface |
 
---
 
## 2.4 Variable Declaration and Assignment, Expression
 
**Variable:** A named memory location used to store a value that can change during program execution.
 
**Declaration:** `dataType variableName;` → e.g. `int age;`
**Assignment:** `variableName = value;` → e.g. `age = 20;`
**Declaration + Assignment (initialization):** `int age = 20;`
 
### Types of Variables in Java
 
| Type | Description |
|---|---|
| **Local variable** | Declared inside a method/block, exists only during method execution |
| **Instance variable** | Declared inside a class but outside methods, belongs to an object |
| **Static variable** | Declared with `static` keyword, shared by all objects of the class |
 
### Expression
A combination of variables, literals, and operators that evaluates to a single value.
 
```java
public class VarDemo {
    static int totalStudents = 0;      // static variable
    int rollNo;                        // instance variable
 
    public static void main(String[] args) {
        int a = 10;                    // local variable
        int b = 5;
        int result = a * b + 2;        // expression -> 52
        System.out.println("Result = " + result);
    }
}
```
 
---
 
## 2.5 Control Statements: Selection, Looping and Jump Statements
 
```
                Control Statements
                        │
      ┌─────────────────┼──────────────────┐
  Selection            Looping             Jump
 (Decision Making)   (Repetition)      (Transfer Control)
      │                  │                   │
   if, if-else,      for, while,        break, continue,
   switch             do-while             return
```
 
### 2.5.1 Selection Statements
 
```java
// if-else example
int marks = 75;
if (marks >= 80) {
    System.out.println("Distinction");
} else if (marks >= 60) {
    System.out.println("First Division");
} else {
    System.out.println("Needs Improvement");
}
 
// switch example
int day = 3;
switch (day) {
    case 1: System.out.println("Sunday"); break;
    case 2: System.out.println("Monday"); break;
    case 3: System.out.println("Tuesday"); break;
    default: System.out.println("Invalid day");
}
```
 
### 2.5.2 Looping Statements
 
```java
// for loop
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
 
// while loop
int i = 1;
while (i <= 5) {
    System.out.println("While Count: " + i);
    i++;
}
 
// do-while loop (executes at least once)
int j = 1;
do {
    System.out.println("Do-While Count: " + j);
    j++;
} while (j <= 5);
```
 
### 2.5.3 Jump Statements
 
| Statement | Purpose |
|---|---|
| `break` | Terminates the loop/switch immediately |
| `continue` | Skips current iteration, moves to next |
| `return` | Exits from the current method, optionally returning a value |
 
```java
for (int i = 1; i <= 10; i++) {
    if (i == 6) break;          // stops loop when i = 6
    if (i % 2 == 0) continue;   // skips even numbers
    System.out.println(i);
}
```
 
---
 
## 2.6 Arrays: Single Dimension Array, Multi-Dimensional Array (Rectangular and Jagged)
 
**Definition:** An array is a collection of fixed-size, same-type elements stored in contiguous memory, accessed using an index (starting from 0).
 
### 2.6.1 Single Dimensional Array
 
```java
public class SingleArrayDemo {
    public static void main(String[] args) {
        int[] marks = {78, 65, 90, 55, 88};   // declaration + initialization
 
        for (int i = 0; i < marks.length; i++) {
            System.out.println("Marks[" + i + "] = " + marks[i]);
        }
    }
}
```
 
### 2.6.2 Multi-Dimensional Array
 
**(a) Rectangular Array** — every row has the same number of columns.
 
```java
public class RectangularArrayDemo {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
 
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```
 
**(b) Jagged Array** — each row can have a different number of columns (array of arrays).
 
```java
public class JaggedArrayDemo {
    public static void main(String[] args) {
        int[][] jagged = new int[3][];
        jagged[0] = new int[]{1, 2};
        jagged[1] = new int[]{3, 4, 5};
        jagged[2] = new int[]{6};
 
        for (int i = 0; i < jagged.length; i++) {
            for (int j = 0; j < jagged[i].length; j++) {
                System.out.print(jagged[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```
 
### Diagram: Rectangular vs Jagged
 
```
Rectangular Array (3x3)          Jagged Array
┌───┬───┬───┐                    ┌───┬───┐
│ 1 │ 2 │ 3 │                    │ 1 │ 2 │
├───┼───┼───┤                    ├───┼───┼───┐
│ 4 │ 5 │ 6 │                    │ 3 │ 4 │ 5 │
├───┼───┼───┤                    ├───┼───┴───┘
│ 7 │ 8 │ 9 │                    │ 6 │
└───┴───┴───┘                    └───┘
(equal columns per row)          (unequal columns per row)
```
 
---
 
## 2.7 Type Conversion and Casting
 
**Definition:** Converting a value from one data type to another.
 
### 2.7.1 Implicit Conversion (Widening) — Automatic
Occurs automatically when converting a **smaller type to a larger type** (no data loss).
 
```java
int a = 100;
double b = a;     // int to double – automatic widening
System.out.println(b);  // 100.0
```
 
`byte → short → int → long → float → double`
 
### 2.7.2 Explicit Conversion (Narrowing) — Casting
Required when converting a **larger type to a smaller type** (possible data loss), must be done manually.
 
```java
double d = 100.99;
int x = (int) d;   // explicit narrowing – decimal part lost
System.out.println(x);  // 100
```
 
### Difference: Widening vs Narrowing
 
| Basis | Widening (Implicit) | Narrowing (Explicit/Casting) |
|---|---|---|
| Direction | Smaller → Larger type | Larger → Smaller type |
| Performed by | Compiler automatically | Programmer manually using `(type)` |
| Data loss | No | Possible |
| Example | `int` to `double` | `double` to `int` |
 
---
 
## 2.8 Garbage Collection
 
**Definition:** Garbage Collection (GC) is the process by which the **JVM automatically reclaims memory** occupied by objects that are no longer reachable/referenced by any part of the program, preventing memory leaks.
 
### Key Points
- Java handles memory management automatically — unlike C/C++ where the programmer must manually `free()`/`delete` memory.
- The garbage collector runs on a **separate low-priority daemon thread**.
- An object becomes eligible for garbage collection when it has **no live references** pointing to it.
- We cannot force GC to run immediately, but we can **request** it using `System.gc()` (JVM may or may not honor it immediately).
- The `finalize()` method (deprecated in newer versions) used to be called by the GC before destroying an object.
### Example
 
```java
public class GCDemo {
    public static void main(String[] args) {
        GCDemo obj1 = new GCDemo();
        obj1 = null;          // object is now unreachable -> eligible for GC
        System.gc();          // request JVM to run garbage collector
        System.out.println("Garbage collection requested.");
    }
 
    @Override
    protected void finalize() {
        System.out.println("Object garbage collected.");
    }
}
```
 
### Diagram
 
```
 Object created  ──►  Referenced by variable  ──►  Reference removed (null)
                                                          │
                                                          ▼
                                              Object becomes "unreachable"
                                                          │
                                                          ▼
                                         Garbage Collector reclaims memory
```
 
---
 
## 2.9 String: Creation, Concatenation, Comparison, Modification, Changing Case and Searching
 
**Definition:** `String` is a class in `java.lang` package representing a sequence of characters. In Java, Strings are **immutable** (once created, the value cannot be changed; any modification creates a new String object).
 
### 2.9.1 Creation
 
```java
String s1 = "Nepal";                  // String literal (stored in String Pool)
String s2 = new String("Nepal");      // using new keyword (stored in Heap)
```
 
### 2.9.2 Concatenation
 
```java
String first = "Butwal";
String second = "Nepal";
String result1 = first + ", " + second;         // using + operator
String result2 = first.concat(", ").concat(second); // using concat()
System.out.println(result1);  // Butwal, Nepal
```
 
### 2.9.3 Comparison
 
| Method | Description |
|---|---|
| `==` | Compares references (memory address), not content |
| `equals()` | Compares actual content (case-sensitive) |
| `equalsIgnoreCase()` | Compares content ignoring case |
| `compareTo()` | Compares lexicographically, returns int (0 = equal) |
 
```java
String a = "java";
String b = "JAVA";
System.out.println(a.equals(b));            // false
System.out.println(a.equalsIgnoreCase(b));  // true
System.out.println(a.compareTo(b));         // non-zero value
```
 
### 2.9.4 Modification (Strings are Immutable)
 
```java
String s = "Hello";
s = s + " World";   // creates a NEW string object; old "Hello" remains unchanged (unreferenced)
System.out.println(s);  // Hello World
```
 
### 2.9.5 Changing Case
 
```java
String name = "Nepathya College";
System.out.println(name.toUpperCase());  // NEPATHYA COLLEGE
System.out.println(name.toLowerCase());  // nepathya college
```
 
### 2.9.6 Searching
 
```java
String text = "Nepal is beautiful";
System.out.println(text.indexOf("is"));       // 6 - position of substring
System.out.println(text.contains("beautiful")); // true
System.out.println(text.charAt(0));           // 'N'
System.out.println(text.substring(0, 5));     // "Nepal"
```
 
---
 
## 2.10 StringBuffer Class
 
**Definition:** `StringBuffer` is a class used to create **mutable** (modifiable) strings — unlike `String`, its content can be changed without creating a new object each time, making it more memory-efficient for frequent modifications. It is also **thread-safe** (synchronized methods).
 
### Difference: String vs StringBuffer
 
| Basis | String | StringBuffer |
|---|---|---|
| Mutability | Immutable | Mutable |
| Memory | New object created on every modification | Modifies same object |
| Performance | Slower for repeated modification | Faster for repeated modification |
| Thread safety | Not applicable (immutable, inherently safe) | Thread-safe (synchronized) |
| Package | `java.lang` | `java.lang` |
 
### Common StringBuffer Methods
 
| Method | Purpose |
|---|---|
| `append()` | Adds text at the end |
| `insert()` | Inserts text at a given position |
| `reverse()` | Reverses the string |
| `delete()` | Deletes characters within a range |
| `replace()` | Replaces characters within a range |
| `capacity()` | Returns current capacity of buffer |
 
### Example
 
```java
public class StringBufferDemo {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer("Nepal");
 
        sb.append(" is beautiful");     // Nepal is beautiful
        System.out.println(sb);
 
        sb.insert(5, " (my country)");  // insert at index 5
        System.out.println(sb);
 
        sb.reverse();                   // reverses entire buffer
        System.out.println(sb);
    }
}
```
 
---