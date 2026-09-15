# Unit 10: Database Programming

## Table of Contents

1. [Database Connectivity](#101-database-connectivity)
2. [Statement Set for Manipulating Data in Database](#102-statement-set-for-manipulating-data-in-database)
3. [ResultSet Interface](#103-resultset-interface)

---

## 10.1 Database Connectivity

**Definition:** **JDBC (Java Database Connectivity)** is an API (part of `java.sql` and `javax.sql` packages) that enables Java programs to connect to, query, and manipulate data in relational databases (such as MySQL, Oracle, PostgreSQL) in a database-independent way, using standard SQL.

### JDBC Architecture

```
   Java Application
          │
     JDBC API (java.sql.*)
          │
    JDBC Driver Manager
          │
     JDBC Driver (database-specific)
          │
      Database (MySQL, Oracle, etc.)
```

| Component | Role |
| --------- | ---- |
| **JDBC API** | Set of interfaces/classes (`Connection`, `Statement`, `ResultSet`, etc.) used by the application |
| **DriverManager** | Manages a list of database drivers; establishes the connection based on the given URL |
| **JDBC Driver** | Database-vendor-specific software that translates JDBC calls into the database's native protocol |
| **Database** | The actual relational database storing the data |

### Types of JDBC Drivers

| Type | Name | Description |
| ---- | ---- | ----------- |
| Type 1 | JDBC-ODBC Bridge | Translates JDBC calls into ODBC calls (obsolete, removed since Java 8) |
| Type 2 | Native-API Driver | Converts JDBC calls into database-specific native calls (requires native libraries) |
| Type 3 | Network Protocol Driver | Sends JDBC calls to a middleware server, which translates them for the database |
| Type 4 | Thin/Pure Java Driver | Converts JDBC calls directly into the database's network protocol; **most commonly used** (pure Java, no native code needed) |

### Steps to Connect Java to a Database

1. **Load/Register the JDBC driver** (mostly automatic in modern JDBC 4.0+ via `ServiceLoader`).
2. **Establish a connection** using `DriverManager.getConnection()`.
3. **Create a `Statement`** to send SQL to the database.
4. **Execute the query/update** and process the results (if any).
5. **Close the connection** and related resources to release them.

### Example: Establishing a Database Connection

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBConnectionDemo {
    public static void main(String[] args) {
        // JDBC URL format: jdbc:<subprotocol>://<host>:<port>/<database_name>
        String url = "jdbc:mysql://localhost:3306/college_db";
        String username = "root";
        String password = "yourpassword";

        try (Connection connection = DriverManager.getConnection(url, username, password)) {
            System.out.println("Database connected successfully!");
            System.out.println("Connection info: " + connection.getMetaData().getDatabaseProductName());
        } catch (SQLException e) {
            System.out.println("Connection failed: " + e.getMessage());
        }
        // try-with-resources automatically closes the connection
    }
}
```

### Key Interfaces/Classes in `java.sql`

| Interface/Class | Purpose |
| ---------------- | ------- |
| `DriverManager` | Manages JDBC drivers and creates connections |
| `Connection` | Represents a session/connection with a specific database |
| `Statement` | Used to execute static SQL statements |
| `PreparedStatement` | Precompiled SQL statement, supports parameters, more efficient and secure |
| `CallableStatement` | Used to execute stored procedures |
| `ResultSet` | Represents the result table returned by a query |
| `SQLException` | Checked exception thrown for database access errors |

---

## 10.2 Statement Set for Manipulating Data in Database

**Definition:** Once a `Connection` is established, SQL statements are sent to the database using one of three interfaces: `Statement`, `PreparedStatement`, or `CallableStatement` — collectively used to **insert, update, delete, and query** data.

### 10.2.1 `Statement` Interface

Used to execute simple, **static** SQL queries (no parameters). Not recommended for repeated queries or queries with user input, due to lower performance and SQL-injection risk.

| Method | Purpose |
| ------ | ------- |
| `executeQuery(String sql)` | Executes a `SELECT` query, returns a `ResultSet` |
| `executeUpdate(String sql)` | Executes `INSERT`, `UPDATE`, `DELETE`, or DDL statements; returns the number of rows affected |
| `execute(String sql)` | Executes any SQL statement; returns `true` if the result is a `ResultSet` |

```java
import java.sql.*;

public class StatementDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college_db";

        try (Connection con = DriverManager.getConnection(url, "root", "yourpassword");
             Statement stmt = con.createStatement()) {

            // INSERT using executeUpdate()
            String insertSQL = "INSERT INTO students (id, name, marks) VALUES (1, 'Manisha KC', 85)";
            int rowsInserted = stmt.executeUpdate(insertSQL);
            System.out.println(rowsInserted + " row(s) inserted");

            // SELECT using executeQuery()
            ResultSet rs = stmt.executeQuery("SELECT * FROM students");
            while (rs.next()) {
                System.out.println(rs.getInt("id") + " - " + rs.getString("name"));
            }

        } catch (SQLException e) {
            System.out.println("Database error: " + e.getMessage());
        }
    }
}
```

### 10.2.2 `PreparedStatement` Interface

A **precompiled** SQL statement that accepts **parameters** (placeholders marked with `?`). Preferred over `Statement` because it is faster for repeated execution and prevents **SQL injection** attacks.

| Method | Purpose |
| ------ | ------- |
| `setInt(index, value)`, `setString(index, value)`, etc. | Binds a value to a `?` placeholder (1-based index) |
| `executeQuery()` | Executes a parameterized `SELECT` |
| `executeUpdate()` | Executes a parameterized `INSERT`/`UPDATE`/`DELETE` |

```java
import java.sql.*;

public class PreparedStatementDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college_db";
        String insertSQL = "INSERT INTO students (id, name, marks) VALUES (?, ?, ?)";

        try (Connection con = DriverManager.getConnection(url, "root", "yourpassword");
             PreparedStatement pstmt = con.prepareStatement(insertSQL)) {

            pstmt.setInt(1, 2);                  // sets value for first '?'
            pstmt.setString(2, "Bishal Thapa");  // sets value for second '?'
            pstmt.setDouble(3, 91.5);            // sets value for third '?'

            int rows = pstmt.executeUpdate();
            System.out.println(rows + " row(s) inserted using PreparedStatement");

        } catch (SQLException e) {
            System.out.println("Database error: " + e.getMessage());
        }
    }
}
```

### 10.2.3 `CallableStatement` Interface

Used to execute **stored procedures** (precompiled SQL routines) residing in the database.

```java
import java.sql.*;

public class CallableStatementDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college_db";
        // Assumes a stored procedure: getStudentById(IN studentId INT)
        String call = "{call getStudentById(?)}";

        try (Connection con = DriverManager.getConnection(url, "root", "yourpassword");
             CallableStatement cstmt = con.prepareCall(call)) {

            cstmt.setInt(1, 1);
            ResultSet rs = cstmt.executeQuery();
            while (rs.next()) {
                System.out.println("Name: " + rs.getString("name"));
            }

        } catch (SQLException e) {
            System.out.println("Database error: " + e.getMessage());
        }
    }
}
```

### Comparison: Statement vs PreparedStatement vs CallableStatement

| Basis | `Statement` | `PreparedStatement` | `CallableStatement` |
| ----- | ----------- | --------------------- | ---------------------- |
| SQL type | Static (no parameters) | Parameterized (precompiled) | Calls stored procedures |
| Performance | Slower for repeated execution | Faster (compiled once, reused) | Depends on the procedure |
| SQL injection risk | High | Low (parameters are escaped safely) | Low |
| Typical use | One-off simple queries | Repeated queries with dynamic values | Executing pre-defined DB procedures |

### Example: UPDATE and DELETE

```java
// UPDATE example
String updateSQL = "UPDATE students SET marks = ? WHERE id = ?";
try (PreparedStatement pstmt = con.prepareStatement(updateSQL)) {
    pstmt.setDouble(1, 95.0);
    pstmt.setInt(2, 1);
    int updatedRows = pstmt.executeUpdate();
    System.out.println(updatedRows + " row(s) updated");
}

// DELETE example
String deleteSQL = "DELETE FROM students WHERE id = ?";
try (PreparedStatement pstmt = con.prepareStatement(deleteSQL)) {
    pstmt.setInt(1, 2);
    int deletedRows = pstmt.executeUpdate();
    System.out.println(deletedRows + " row(s) deleted");
}
```

---

## 10.3 ResultSet Interface

**Definition:** `ResultSet` represents the **table of data** returned by executing a `SELECT` query (via `executeQuery()`). It maintains a **cursor** pointing to the current row of data, initially positioned **before the first row**.

### Navigating a `ResultSet`

| Method | Purpose |
| ------ | ------- |
| `next()` | Moves the cursor to the next row; returns `false` when no more rows exist |
| `previous()` | Moves the cursor to the previous row (requires a scrollable `ResultSet`) |
| `first()` / `last()` | Moves the cursor to the first/last row |
| `beforeFirst()` / `afterLast()` | Moves the cursor before the first row / after the last row |
| `absolute(int row)` | Moves the cursor to a specific row number |

### Retrieving Column Data

| Method | Purpose |
| ------ | ------- |
| `getInt(columnName / index)` | Retrieves an `int` value from a column |
| `getString(columnName / index)` | Retrieves a `String` value |
| `getDouble(columnName / index)` | Retrieves a `double` value |
| `getDate(columnName / index)` | Retrieves a `java.sql.Date` value |
| `getObject(columnName / index)` | Retrieves the value as a generic `Object` |

### Example: Iterating a `ResultSet`

```java
import java.sql.*;

public class ResultSetDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college_db";
        String query = "SELECT id, name, marks FROM students";

        try (Connection con = DriverManager.getConnection(url, "root", "yourpassword");
             Statement stmt = con.createStatement();
             ResultSet rs = stmt.executeQuery(query)) {

            System.out.println("ID\tName\t\tMarks");
            while (rs.next()) {                 // cursor moves to next row each iteration
                int id = rs.getInt("id");
                String name = rs.getString("name");
                double marks = rs.getDouble("marks");
                System.out.println(id + "\t" + name + "\t" + marks);
            }

        } catch (SQLException e) {
            System.out.println("Database error: " + e.getMessage());
        }
    }
}
```

### `ResultSet` Types (Scrollability and Updatability)

By default, a `ResultSet` is **forward-only** and **read-only**. Different types can be requested when creating the `Statement`:

```java
Statement stmt = con.createStatement(
    ResultSet.TYPE_SCROLL_INSENSITIVE,   // scrollability
    ResultSet.CONCUR_UPDATABLE           // concurrency (updatability)
);
```

| Type Constant | Description |
| ------------- | ----------- |
| `TYPE_FORWARD_ONLY` | Cursor moves forward only (default) |
| `TYPE_SCROLL_INSENSITIVE` | Cursor can move in both directions; does not reflect changes made by others after the query executed |
| `TYPE_SCROLL_SENSITIVE` | Cursor can move in both directions; reflects changes made by others in real time |
| `CONCUR_READ_ONLY` | `ResultSet` cannot be used to update the database (default) |
| `CONCUR_UPDATABLE` | `ResultSet` can be used to update rows directly |

### Example: Updating Data via an Updatable `ResultSet`

```java
import java.sql.*;

public class UpdatableResultSetDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college_db";

        try (Connection con = DriverManager.getConnection(url, "root", "yourpassword");
             Statement stmt = con.createStatement(
                     ResultSet.TYPE_SCROLL_SENSITIVE,
                     ResultSet.CONCUR_UPDATABLE);
             ResultSet rs = stmt.executeQuery("SELECT * FROM students")) {

            while (rs.next()) {
                if (rs.getString("name").equals("Manisha KC")) {
                    rs.updateDouble("marks", 98.0);   // update the column value in memory
                    rs.updateRow();                     // commits the update to the database
                    System.out.println("Record updated for Manisha KC");
                }
            }

        } catch (SQLException e) {
            System.out.println("Database error: " + e.getMessage());
        }
    }
}
```

### `ResultSetMetaData` — Inspecting Result Structure

`ResultSetMetaData` provides information **about** the `ResultSet` itself (column names, types, count) — useful for building generic/dynamic database tools.

```java
ResultSetMetaData metaData = rs.getMetaData();
int columnCount = metaData.getColumnCount();
for (int i = 1; i <= columnCount; i++) {
    System.out.println("Column " + i + ": " + metaData.getColumnName(i)
                        + " (" + metaData.getColumnTypeName(i) + ")");
}
```

---