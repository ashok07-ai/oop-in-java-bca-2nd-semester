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
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamExample {

    public static void main(String[] args) throws IOException {

        FileOutputStream fos = new FileOutputStream("nepathya.txt");

        String text = "Nepathya is affiliated to Tribhuvan University \n We have two faculties BCA and BSc.CSIT";

        fos.write(text.getBytes()); // "hello java" -> getBytes() -> byte array

        fos.close();

        System.out.println("file created and data added successfully");
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

public class FileInputStreamExample {

    public static void main(String[] args) throws IOException {

        FileInputStream fis = new FileInputStream("nepathya.txt");

        int data;

        while ((data = fis.read()) != -1) {
            System.out.print((char) data);
        }

        fis.close();
    }
}
```
 
### Program for Downloading image using Byte Stream class
 
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DownloadImageExample {
  public static void main(String[] args) throws IOException {
    FileInputStream fis = new FileInputStream("thor.jpeg");
    FileOutputStream fos = new FileOutputStream("thor_backup.jpeg");

    int data;

    while((data = fis.read()) != -1){
      fos.write(data);
    }

    fis.close();
    fos.close();
    System.out.println("Photo copied successfully...");
  }
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
import java.io.IOException;

public class FileWriterExample {
  public static void main(String[] args) throws IOException{
    FileWriter writer = new FileWriter("abc.txt"); 

    writer.write("Java is a programming language\nLatest version of java is 26.0");
    writer.write("\n");
    writer.write("C is a programming language");
    writer.write("\n");
    writer.write("BCA, Bsc.CSIT");

    writer.close();
    System.out.println("File created successfully!!");
  }
}

```
 
### Example: Reading Characters from a File (line by line)
 
```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderExample {
  public static void main(String[] args) throws IOException {
    FileReader fileReader = new FileReader("abc.txt");

    int data;

    while((data = fileReader.read()) != -1){
      System.out.print((char) data);
    }

    fileReader.close();
    System.out.println("File read successfully....");
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
    String name;
    int rollNo;
 
    Student(String name, int rollNo) {
        this.name = name;
        this.rollNo = rollNo;
    }
}
```
 
### Serializing (Writing) an Object
 
```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class ObjectOutputStreamExample{
  public static void main(String[] args) throws FileNotFoundException, IOException {
    // Create student object
    Student std = new Student("Alpha", 20);

    // Create a binary file that stores the data of an object
    FileOutputStream fos = new FileOutputStream("student.dat");

    // Store the object in the created file -> student.dat
    ObjectOutputStream oos = new ObjectOutputStream(fos);

    oos.writeObject(std);

    oos.close();
    fos.close();

    System.out.println("Object saved successfully!!");
  }
}
```
 
### Deserializing (Reading) an Object
 
```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class ObjectInputStreamClassExample {
  public static void main(String[] args) throws Exception {

    FileInputStream fis = new FileInputStream("student.dat");

    ObjectInputStream ois = new ObjectInputStream(fis);

    Student std = (Student) ois.readObject();

    // display the details
    System.out.println("Name: " + std.name);
    System.out.println("Age" + std.age);

    // close refrences
    ois.close();
    fis.close();

    System.out.println("File reterived successfully");

  }
}
```
 
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
import java.io.IOException;
import java.io.RandomAccessFile;

public class RandomAccessFileExample {
  public static void main(String[] args) throws IOException {
    RandomAccessFile file = new RandomAccessFile("xyz.txt", "rw");

    file.writeUTF("Hello.java");

    // Move to the beginning
    file.seek(0);

    System.out.println(file.readUTF());

    file.close();
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
