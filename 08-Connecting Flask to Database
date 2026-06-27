# Chapter 8: Data Persistence

## 8.1 Introduction

Until now, our Flask applications could receive user input through forms, but that data existed only during the current request. Once the application stopped or the server restarted, the data was lost.

To store application data permanently, Flask uses a **database**.

A database provides persistent storage, allowing applications to save and retrieve information whenever required. Examples include user accounts, products, orders, attendance records, and messages.

For this chapter, we'll use **SQLite** because it is lightweight, built into Python, requires no separate installation, and stores the entire database in a single `.db` file.

---

# 8.2 Why SQLite?

Flask supports multiple databases, but SQLite is the preferred choice for beginners because:

- It is built into Python (`sqlite3` module).
- No separate installation or database server is required.
- It automatically creates the database file if it doesn't exist.
- It is lightweight and ideal for learning, small projects, and prototypes.

---

# 8.3 Importing SQLite

Python provides a built-in module called `sqlite3` to interact with SQLite databases.

```python
import sqlite3
```

No additional installation is required.

---

# 8.4 Connecting to the Database

To connect to an existing database or create a new one:

```python
conn = sqlite3.connect("students.db")
```

Here:

- `conn` is a Python variable that stores the database connection object.
- `"students.db"` is the database file name. You can choose any meaningful name.

If the database already exists, SQLite connects to it. Otherwise, it automatically creates the database.

---

# 8.5 The Connection Object

The object returned by `sqlite3.connect()` is called the **Connection Object**.

```python
conn = sqlite3.connect("students.db")
```

The connection object manages communication between the Python application and the database.

Common methods provided by the connection object are:

- `cursor()`
- `commit()`
- `close()`

---

# 8.6 The Cursor Object

A cursor is responsible for executing SQL queries on the connected database.

```python
cursor = conn.cursor()
```

Think of the cursor as the interface through which Python sends SQL commands to the database.

Without creating a cursor, SQL queries cannot be executed.

---

# 8.7 Executing SQL Queries

SQL statements are executed using the cursor's `execute()` method.

Example:

```python
cursor.execute("""
CREATE TABLE students(
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
)
""")
```

Notice that:

- Triple quotes (`"""`) are Python syntax used to write multi-line strings.
- The SQL query is written inside those triple quotes.

---

# 8.8 Saving Changes (`commit()`)

Whenever the database is modified, the changes must be saved.

```python
conn.commit()
```

`commit()` is required for operations such as:

- CREATE
- INSERT
- UPDATE
- DELETE

It is **not required** for `SELECT` queries because they only retrieve data.

---

# 8.9 Closing the Connection (`close()`)

After completing all database operations, close the database connection.

```python
conn.close()
```

Closing the connection releases the resources used by the database and is considered a best practice.

---

# 8.10 Fetching Data

After executing a `SELECT` query, SQLite provides three methods to retrieve the results.

## `fetchone()`

Returns a single row.

```python
student = cursor.fetchone()
```

Used when only one record is expected.

---

## `fetchmany(n)`

Returns the specified number of rows.

```python
students = cursor.fetchmany(5)
```

Used when retrieving a limited number of records.

---

## `fetchall()`

Returns all remaining rows.

```python
students = cursor.fetchall()
```

Used when all matching records are required.

---

# 8.11 Parameterized Queries

Instead of directly placing values inside SQL queries, SQLite uses **placeholders (`?`)**.

Example:

```python
cursor.execute(
    "SELECT * FROM students WHERE id = ?",
    (student_id,)
)
```

Here:

- `?` is a placeholder.
- `(student_id,)` is a tuple containing the value that replaces the placeholder.

If multiple values are required, use multiple placeholders.

```python
cursor.execute(
    "SELECT * FROM students WHERE id = ? AND name = ?",
    (student_id, name)
)
```

Parameterized queries are the recommended approach because they safely pass values to SQL queries and make the query reusable.

---

# 8.12 Complete SQLite Workflow

A typical SQLite program follows the same sequence.

```python
import sqlite3

# Connect to the database
conn = sqlite3.connect("students.db")

# Create a cursor
cursor = conn.cursor()

# Execute SQL query
cursor.execute("""
CREATE TABLE IF NOT EXISTS students(
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
)
""")

# Save changes
conn.commit()

# Close the connection
conn.close()
```

General workflow:

```text
Import
    ↓
Connect
    ↓
Create Cursor
    ↓
Execute SQL Query
    ↓
Commit (if data changes)
    ↓
Close Connection
```

---

# Chapter Summary

In this chapter, you learned how Flask applications interact with SQLite databases.

### Key Takeaways

- SQLite is the default database used for learning Flask because it is lightweight and built into Python.
- `sqlite3.connect()` creates or connects to a database.
- The **Connection Object** manages communication with the database.
- The **Cursor Object** executes SQL queries.
- `execute()` runs SQL statements.
- `commit()` permanently saves changes made to the database.
- `close()` closes the database connection and releases resources.
- `fetchone()`, `fetchmany()`, and `fetchall()` retrieve query results.
- Parameterized queries use placeholders (`?`) to safely pass values into SQL statements.

With these concepts, your Flask application can now store and retrieve data permanently. In the next chapter, you'll combine routing, templates, forms, and databases to build a complete Flask application.