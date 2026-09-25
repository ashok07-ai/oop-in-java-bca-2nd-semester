# Object Oriented Programming in Java — BCA 2nd Semester

Personal study notes for the **Object Oriented Programming using Java** course, written for the BCA 2nd Semester (Tribhuvan University) syllabus. Notes are organized unit-wise, with definitions, diagrams, tables, and runnable Java examples for every topic.

## 📚 Units

| Unit | Topic | Notes |
|---|---|---|
| 1 | Introduction to Java and OOP Concepts | [1_Introduction_to_Java_and_OOP_concept.md](1_Introduction_to_Java_and_OOP_concept.md) |
| 2 | Basics of Java Programming | [2_Basic_of_Java_Programming.md](2_Basic_of_Java_Programming.md) |
| 3 | Class and Objects in Java | [3_Class_and_Objects_in_Java.md](3_Class_and_Objects_in_Java.md) |
| 4 | Inheritance and Polymorphism | [4_Inheritance_and_Polymorphism.md](4_Inheritance_and_Polymorphism.md) |
| 5 | Exception Handling and Multithreading | [5_Exception_Handeling_and_Multithreading.md](5_Exception_Handeling_and_Multithreading.md) |
| 6 | File Handling in Java | [6_File_Handeling_in_Java.md](6_File_Handeling_in_Java.md) |

### What's inside each unit

- **Unit 1** — Java history, JVM/JRE/JDK, POP vs OOP, the four pillars of OOP, compiling & running programs, command-line arguments, `Scanner`, and error types.
- **Unit 2** — Comments, tokens, data types, variables, control statements, arrays, type casting, garbage collection, `String` and `StringBuffer`.
- **Unit 3** — Classes & objects, abstraction/encapsulation, constructors, `this`, static members, pass-by-value, recursion, nested classes, varargs, packages.
- **Unit 4** — Inheritance types, `super`, method overloading/overriding, the `Object` class, `final`, abstract classes, access modifiers, interfaces.
- **Unit 5** — Exceptions, exception hierarchy, `try/catch/finally`, custom exceptions, multithreading basics, `Thread`/`Runnable`, thread priorities, synchronization.
- **Unit 6** — Console & file I/O, byte streams, character streams, serialization/deserialization, `RandomAccessFile`.

## 🖼️ Images

Diagrams referenced in the notes live in [`images/`](images/). Currently used:

| Image | Used in |
|---|---|
| `architecture_of_jvm.png` | Unit 1 – JVM execution architecture |
| `class-and-objects.png` | Unit 3 – Class vs Object |
| `inheritance.png` | Unit 4 – Types of inheritance |
| `lifecycle_of_thread.png` | Unit 5 – Thread life cycle |

A few sections still rely on ASCII-art diagrams and would benefit from real images — see the open items below if you'd like to contribute one.

**Suggested additions:**
- `data-types-hierarchy.png` — Unit 2, §2.3 (Primitive vs Non-Primitive types)
- `array-rectangular-vs-jagged.png` — Unit 2, §2.6
- `io-streams-hierarchy.png` — Unit 6, §6.1 (Byte vs Character streams)
- `serialization-flow.png` — Unit 6, §6.4
- `exception-hierarchy.png` — Unit 5, §5.2
- `overloading-vs-overriding.png` — Unit 4, §4.4

## 📁 Repository Structure

```
oop-in-java-bca-2nd-semester/
├── images/                                        # Diagrams used across the notes
├── rju/                                            # Additional / Rajarshi Janak University (RJU) resources
├── tu/                                             # Additional / Tribhuvan University (TU) resources
├── 1_Introduction_to_Java_and_OOP_concept.md
├── 2_Basic_of_Java_Programming.md
├── 3_Class_and_Objects_in_Java.md
├── 4_Inheritance_and_Polymorphism.md
├── 5_Exception_Handeling_and_Multithreading.md
├── 6_File_Handeling_in_Java.md
└── README.md
```

## 🚀 How to Use

1. Browse the unit files above directly on GitHub, or clone the repo:
   ```bash
   git clone https://github.com/ashok07-ai/oop-in-java-bca-2nd-semester.git
   ```
2. Each unit is self-contained with theory, tables, diagrams, and full runnable Java examples (compile with `javac` and run with `java` as shown in Unit 1).
3. Use the **Table of Contents** at the top of each `.md` file to jump to a specific topic.

## 🤝 Contributing

Found an error, want to add a missing diagram, or have a clearer example? Feel free to open an issue or a pull request.

