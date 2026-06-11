# Edutech Solution Python Development Internship

## Task 11: Database Integration

## Files
- `main.py`
- `database.db`
- `database.sql`
- `README.md`
- `image.png`
---

## Guide: Connecting Python to Database & CRUD Operations

### 1. Connecting Python to SQLite
In Python, database connectivity is established using a connection object.
```python
import sqlite3
conn = sqlite3.connect("database.db")  # Creates the database file if it does not exist
cursor = conn.cursor()                  # Creates a cursor object to execute queries
```

### 2. Executing CRUD Operations
- **CREATE (Insert Data):** Add new registration rows into the database.
  ```sql
  INSERT INTO students (name, course) VALUES (?, ?)
  ```
- **READ (Retrieve Data):** Query the database using `SELECT` statements and fetch the results.
  ```sql
  SELECT * FROM students
  ```
- **UPDATE (Modify Data):** Change values in existing records based on specific criteria.
  ```sql
  UPDATE students SET course = ? WHERE name = ?
  ```
- **DELETE (Remove Data):** Delete specific records from the table.
  ```sql
  DELETE FROM students WHERE name = ?
  ```

---

## 💬 Interview Questions & Deliverables

### Q1. How does a developer/intern bridge Python with database storage?
To connect a Python application to a persistent storage system, developers use **Database Drivers or Adapters** and **Connection Protocols**:
- **Database Drivers:** Libraries like `sqlite3` (built-in for SQLite), `mysql-connector-python` or `PyMySQL` (for MySQL), and `psycopg2` (for PostgreSQL) translate Python database API calls into native database protocols.
- **Connection Lifecycle:** 
  1. **Connection Object:** Establishes a communication channel between Python and the database engine.
  2. **Cursor Object:** Used to execute SQL statements and retrieve results.
  3. **Transactions:** Commits (`conn.commit()`) save changes permanently, while rollbacks (`conn.rollback()`) discard changes if an error occurs.
  4. **Closing:** Always close connections (`conn.close()`) to free up memory and system resources.
- **Object-Relational Mapping (ORMs):** For complex projects, developers often use ORMs like *SQLAlchemy* or *Django ORM* to map database tables directly to Python classes, allowing them to interact with the database using pure Python object-oriented code instead of raw SQL queries.

### Q2. What is CRUD?
**CRUD** stands for the four basic operations of persistent storage:
1. **Create:** Adding new records to the database (SQL: `INSERT`).
2. **Read (or Retrieve):** Fetching existing records based on search criteria (SQL: `SELECT`).
3. **Update:** Modifying details of existing records (SQL: `UPDATE`).
4. **Delete:** Removing records from the database table (SQL: `DELETE`).

### Q3. Why use database cursors?
A **Cursor** is a database control structure that enables traversal over the records in a database.
- **Row-by-Row Processing:** It fetches query results one row (or a subset of rows) at a time, preventing high memory usage when querying large datasets.
- **Execution Context:** It acts as a middleman that compiles and executes SQL queries, holds the result set, and tracks the current position within the results.
- **Security:** Using cursors with parameterized queries (e.g., `cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))`) prevents SQL Injection attacks.

---

## 🚀 Code and Output

### 1. Python Code (`main.py`)
```python
try:
    import sqlite3
    
    # Connect to SQLite database (creates 'database.db' file)
    conn = sqlite3.connect("database.db")
    cursor = conn.cursor()
    
    # Create the students table
    cursor.execute("""
    CREATE TABLE IF NOT EXISTS students (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        course TEXT
    )
    """)
    
    # Check if the record already exists to prevent duplicate entries on rerun
    cursor.execute("SELECT COUNT(*) FROM students WHERE name = 'Shrey Gamit' AND course = 'Python Internship'")
    if cursor.fetchone()[0] == 0:
        cursor.execute(
            "INSERT INTO students (name, course) VALUES (?, ?)",
            ("Shrey Gamit", "Python Internship")
        )
        conn.commit()
    
    # Retrieve and print the data
    cursor.execute("SELECT * FROM students")
    for row in cursor.fetchall():
        print(row)
        
    # Generate SQL dump
    with open("database.sql", "w", encoding="utf-8") as f:
        for line in conn.iterdump():
            f.write(f"{line}\n")
            
    conn.close()

except Exception as e:
    print("Error:", e)
```

### 2. Terminal Output
Running the script yields:
```text
(1, 'Shrey Gamit', 'Python Internship')
```

### 3. Generated SQL Dump (`database.sql`)
```sql
BEGIN TRANSACTION;
CREATE TABLE students (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        course TEXT
    );
INSERT INTO "students" VALUES(1,'Shrey Gamit','Python Internship');
DELETE FROM "sqlite_sequence";
INSERT INTO "sqlite_sequence" VALUES('students',1);
COMMIT;
```


