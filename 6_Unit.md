 # Unit 6: File Handling in Java

## Table of Contents
 
1. [Console and File I/O](#61-console-and-file-io)
2. [Reading and Writing File using Byte Stream](#62-reading-and-writing-file-using-byte-stream)
3. [Reading and Writing File using Character Stream](#63-reading-and-writing-file-using-character-stream)
4. [Serialization and Deserialization](#64-serialization-and-deserialization)
5. [RandomAccessFile Class](#65-randomaccessfile-class)
---
 
## 6.1 Console and File I/O
 
**Definition:** Java handles Input/Output (I/O) operations using the concept of **streams** — a sequence of data. Streams can read data from a **source** (input stream) or write data to a **destination** (output stream), whether that is the console, a file, memory, or a network connection. All I/O classes are found in the `java.io` package.
 
### Stream Classification
 
```
                     Streams
                        │
        ┌───────────────┴───────────────┐
   Byte Stream                    Character Stream
 (handles raw binary data,       (handles text data,
  8-bit bytes; classes end       16-bit Unicode; classes
  with "Stream")                  end with "Reader"/"Writer")
```
 
| Basis | Byte Stream | Character Stream |
| ----- | ----------- | ------------------ |
| Unit of data | 8-bit byte | 16-bit Unicode character |
| Base classes | `InputStream`, `OutputStream` | `Reader`, `Writer` |
| Use case | Images, audio, video, binary files | Text files |
 
### Console I/O
 
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
 
public class ConsoleIODemo {
    public static void main(String[] args) throws IOException {
        // Reading from console using BufferedReader wrapped over InputStreamReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.print("Enter your name: ");
        String name = br.readLine();
        System.out.println("Hello, " + name + "!");
 
        // Alternative - using Scanner class (java.util)
        // Scanner sc = new Scanner(System.in);
        // String name = sc.nextLine();
    }
}
```
 
### The `File` Class
 
`File` represents a file or directory pathname; it does not itself read/write file content but is used to create, delete, or query file/directory properties.
 
```java
import java.io.File;
import java.io.IOException;
 
public class FileClassDemo {
    public static void main(String[] args) throws IOException {
        File file = new File("student.txt");
 
        if (!file.exists()) {
            file.createNewFile();               // creates a new empty file
            System.out.println("File created: " + file.getName());
        }
 
        System.out.println("Absolute Path: " + file.getAbsolutePath());
        System.out.println("Is Directory? " + file.isDirectory());
        System.out.println("File Size (bytes): " + file.length());
        System.out.println("Can Read? " + file.canRead());
        System.out.println("Can Write? " + file.canWrite());
    }
}
```
 
---
 
## 6.2 Reading and Writing File using Byte Stream
 
**Definition:** Byte streams (`FileInputStream`, `FileOutputStream`) are used to perform input and output of raw 8-bit bytes, mainly for **binary files** (images, audio, executable files), though they can also be used with text files.
 
### Key Classes
 
| Class | Purpose |
| ----- | ------- |
| `FileInputStream` | Reads raw bytes from a file |
| `FileOutputStream` | Writes raw bytes to a file |
 
### Example: Writing Bytes to a File
 
```java
import java.io.FileOutputStream;
import java.io.IOException;
 
public class ByteWriteDemo {
    public static void main(String[] args) {
        String data = "Learning Byte Streams in Java";
        try (FileOutputStream fos = new FileOutputStream("bytefile.txt")) {
            byte[] byteArray = data.getBytes();     // converts String to byte array
            fos.write(byteArray);
            System.out.println("Data written successfully using FileOutputStream");
        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }
}
```
 
### Example: Reading Bytes from a File
 
```java
import java.io.FileInputStream;
import java.io.IOException;
 
public class ByteReadDemo {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("bytefile.txt")) {
            int byteData;
            while ((byteData = fis.read()) != -1) {    // read() returns -1 at end of file
                System.out.print((char) byteData);
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }
}
```
 
### Reading in Chunks (using a buffer array)
 
```java
try (FileInputStream fis = new FileInputStream("bytefile.txt")) {
    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = fis.read(buffer)) != -1) {
        System.out.println(new String(buffer, 0, bytesRead));
    }
} catch (IOException e) {
    System.out.println("Error: " + e.getMessage());
}
```
 
---
 
## 6.3 Reading and Writing File using Character Stream
 
**Definition:** Character streams (`FileReader`, `FileWriter`) are designed to handle **text data** using 16-bit Unicode characters, making them suitable for reading/writing text files, especially across different languages/encodings.
 
### Key Classes
 
| Class | Purpose |
| ----- | ------- |
| `FileReader` | Reads character data from a file |
| `FileWriter` | Writes character data to a file |
| `BufferedReader` | Wraps a `Reader` to read text efficiently, line by line |
| `BufferedWriter` | Wraps a `Writer` to write text efficiently |
 
### Example: Writing Characters to a File
 
```java
import java.io.FileWriter;
import java.io.BufferedWriter;
import java.io.IOException;
 
public class CharWriteDemo {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("notes.txt"))) {
            bw.write("Unit 6: File Handling in Java");
            bw.newLine();                     // writes a line separator
            bw.write("Character streams handle text efficiently.");
            System.out.println("File written successfully using FileWriter");
        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }
}
```
 
### Example: Reading Characters from a File (line by line)
 
```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;
 
public class CharReadDemo {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("notes.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {   // readLine() returns null at end of file
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }
}
```
 
### Byte Stream vs Character Stream — Summary
 
| Basis | Byte Stream | Character Stream |
| ----- | ----------- | ------------------ |
| Classes | `FileInputStream` / `FileOutputStream` | `FileReader` / `FileWriter` |
| Data handled | Binary data (images, audio, video) | Text data (Unicode characters) |
| Efficiency for text | Less efficient | More efficient (handles character encoding) |
 
---
 
## 6.4 Serialization and Deserialization
 
**Definition:** **Serialization** is the process of converting an object's state into a **byte stream** so it can be saved to a file, sent over a network, or stored in memory. **Deserialization** is the reverse process — reconstructing the object from the byte stream. A class must implement the marker interface **`Serializable`** (`java.io.Serializable`) to be eligible for serialization.
 
### Key Classes
 
| Class | Purpose |
| ----- | ------- |
| `ObjectOutputStream` | Writes (serializes) an object to a stream |
| `ObjectInputStream` | Reads (deserializes) an object from a stream |
| `Serializable` | Marker interface (no methods) — a class must implement this to be serializable |
 
### Example (Student Record)
 
```java
import java.io.Serializable;
 
class Student implements Serializable {
    private static final long serialVersionUID = 1L;   // version control for serialized class
    String name;
    int rollNo;
    transient String password;   // 'transient' fields are NOT serialized
 
    Student(String name, int rollNo, String password) {
        this.name = name;
        this.rollNo = rollNo;
        this.password = password;
    }
}
```
 
### Serializing (Writing) an Object
 
```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.IOException;
 
public class SerializeDemo {
    public static void main(String[] args) {
        Student s1 = new Student("Manisha KC", 21, "secret123");
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(s1);            // serializes the object to student.ser
            System.out.println("Object serialized successfully");
        } catch (IOException e) {
            System.out.println("Serialization error: " + e.getMessage());
        }
    }
}
```
 
### Deserializing (Reading) an Object
 
```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;
import java.io.IOException;
 
public class DeserializeDemo {
    public static void main(String[] args) {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student s1 = (Student) ois.readObject();     // deserializes the object
            System.out.println("Name: " + s1.name);
            System.out.println("Roll No: " + s1.rollNo);
            System.out.println("Password: " + s1.password);   // will print 'null' - was transient
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Deserialization error: " + e.getMessage());
        }
    }
}
```
 
### Important Notes
 
- The `transient` keyword excludes a field from being serialized (useful for sensitive data like passwords, or non-serializable fields).
- `serialVersionUID` is used to verify that the sender and receiver of a serialized object have loaded compatible classes; mismatches throw `InvalidClassException`.
- If a class contains a reference to another object, that referenced class must also implement `Serializable`, or the field must be marked `transient`.
---
 
## 6.5 RandomAccessFile Class
 
**Definition:** `RandomAccessFile` allows both **reading and writing** to a file at any arbitrary position, unlike the sequential streams discussed earlier. It treats the file as a large array of bytes, with a **file pointer** that can be moved to any location for reading or writing.
 
### Modes
 
| Mode | Description |
| ---- | ----------- |
| `"r"` | Read-only access |
| `"rw"` | Read and write access (creates the file if it doesn't exist) |
 
### Important Methods
 
| Method | Purpose |
| ------ | ------- |
| `seek(long pos)` | Moves the file pointer to the specified byte position |
| `getFilePointer()` | Returns the current position of the file pointer |
| `read()` / `write()` | Reads/writes data at the current pointer position |
| `readInt()`, `readUTF()`, `writeInt()`, `writeUTF()`, etc. | Type-specific read/write methods |
| `length()` | Returns the length of the file |
 
### Example: Writing and Randomly Accessing Data
 
```java
import java.io.RandomAccessFile;
import java.io.IOException;
 
public class RandomAccessFileDemo {
    public static void main(String[] args) {
        try (RandomAccessFile raf = new RandomAccessFile("records.dat", "rw")) {
            // Writing records - each record has fixed size: int (4 bytes) + UTF name
            raf.writeInt(101);
            raf.writeUTF("Alpha alpha");
 
            raf.writeInt(102);
            raf.writeUTF("Beta beta");
 
            // Move the file pointer back to the beginning to read the first record
            raf.seek(0);
            int id1 = raf.readInt();
            String name1 = raf.readUTF();
            System.out.println("Record 1 -> ID: " + id1 + ", Name: " + name1);
 
            // Jump directly to the second record without reading sequentially
            long secondRecordPosition = 4 + (2 + name1.length()); // int size + UTF length
            raf.seek(secondRecordPosition);
            int id2 = raf.readInt();
            String name2 = raf.readUTF();
            System.out.println("Record 2 -> ID: " + id2 + ", Name: " + name2);
 
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
 
### RandomAccessFile vs Sequential Streams
 
| Basis | Sequential Streams (`FileInputStream`/`FileReader`) | `RandomAccessFile` |
| ----- | ----------------------------------------------------- | -------------------- |
| Access pattern | Sequential only (start to end) | Random - can jump to any position |
| Read + Write | Requires separate input/output stream classes | Single class supports both |
| Use case | Simple sequential file processing | Databases, fixed-length record files, indexed access |
 
---