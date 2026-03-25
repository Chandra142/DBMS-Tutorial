# DBMS Tutorial

A comprehensive reference and tutorial guide covering core **Database Management System (DBMS)** concepts, from fundamentals to advanced topics.

---

## 📚 Table of Contents

1. [Introduction to DBMS](#introduction-to-dbms)
2. [Types of Databases](#types-of-databases)
3. [Data Models](#data-models)
4. [Relational Database Concepts](#relational-database-concepts)
5. [SQL Basics](#sql-basics)
6. [Advanced SQL](#advanced-sql)
7. [Normalization](#normalization)
8. [Transactions & ACID Properties](#transactions--acid-properties)
9. [Indexing](#indexing)
10. [Concurrency Control](#concurrency-control)
11. [Database Security](#database-security)
12. [NoSQL Databases](#nosql-databases)
13. [Contributing](#contributing)
14. [License](#license)

---

## Introduction to DBMS

A **Database Management System (DBMS)** is software that enables users to create, manage, and interact with databases. It provides an efficient, reliable, and secure way to store and retrieve data.

### Key Functions
- **Data Definition** – Define the structure/schema of data.
- **Data Manipulation** – Insert, update, delete, and retrieve data.
- **Data Security** – Control access and protect data integrity.
- **Data Recovery** – Back up and restore data after failures.

### Popular DBMS Software
| DBMS | Type | Use Case |
|------|------|----------|
| MySQL | Relational | Web applications |
| PostgreSQL | Relational | Enterprise / complex queries |
| Oracle DB | Relational | Large enterprise systems |
| MongoDB | Document (NoSQL) | Flexible, schema-less data |
| SQLite | Relational (embedded) | Mobile / local storage |

---

## Types of Databases

| Type | Description | Example |
|------|-------------|---------|
| **Relational (RDBMS)** | Data stored in tables with rows and columns | MySQL, PostgreSQL |
| **Hierarchical** | Data organized in a tree-like structure | IBM IMS |
| **Network** | Data organized as records connected via links | IDMS |
| **Object-Oriented** | Data stored as objects (like OOP) | db4o |
| **NoSQL** | Non-tabular, flexible schemas | MongoDB, Cassandra |
| **NewSQL** | Scalable RDBMS with NoSQL performance | CockroachDB, Spanner |

---

## Data Models

A **data model** defines how data is structured, stored, and manipulated.

- **Relational Model** – Uses tables (relations). Most widely used.
- **Entity-Relationship (ER) Model** – Visualizes entities and their relationships.
- **Hierarchical Model** – Parent-child relationships in a tree structure.
- **Network Model** – Many-to-many relationships using graph structure.
- **Object-Relational Model** – Combines relational and object-oriented concepts.

---

## Relational Database Concepts

### Key Terminology
| Term | Definition |
|------|-----------|
| **Table (Relation)** | Collection of rows and columns |
| **Row (Tuple)** | A single record in a table |
| **Column (Attribute)** | A field that describes a property |
| **Primary Key** | Uniquely identifies each row in a table |
| **Foreign Key** | Links two tables by referencing a primary key |
| **Schema** | The structure/blueprint of the database |

### Keys
- **Primary Key**: Uniquely identifies a record. Cannot be NULL.
- **Foreign Key**: References a primary key in another table.
- **Candidate Key**: A minimal set of attributes that can uniquely identify a row.
- **Composite Key**: A primary key composed of two or more columns.
- **Super Key**: Any set of attributes that uniquely identifies a row.

---

## SQL Basics

**SQL (Structured Query Language)** is the standard language for interacting with relational databases.

### Categories of SQL Commands
| Category | Commands |
|----------|----------|
| DDL (Data Definition) | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| DML (Data Manipulation) | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| DCL (Data Control) | `GRANT`, `REVOKE` |
| TCL (Transaction Control) | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

### Examples

**Create a Table**
```sql
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name       VARCHAR(100) NOT NULL,
    email      VARCHAR(100) UNIQUE,
    age        INT,
    dept_id    INT
);
```

**Insert Data**
```sql
INSERT INTO Students (student_id, name, email, age, dept_id)
VALUES (1, 'Alice', 'alice@example.com', 20, 101);
```

**Query Data**
```sql
SELECT name, age FROM Students WHERE age > 18 ORDER BY name;
```

**Update Data**
```sql
UPDATE Students SET age = 21 WHERE student_id = 1;
```

**Delete Data**
```sql
DELETE FROM Students WHERE student_id = 1;
```

---

## Advanced SQL

### Joins
```sql
-- INNER JOIN: returns rows with matching values in both tables
SELECT s.name, d.dept_name
FROM Students s
INNER JOIN Departments d ON s.dept_id = d.dept_id;

-- LEFT JOIN: all rows from the left table, matched rows from the right
SELECT s.name, d.dept_name
FROM Students s
LEFT JOIN Departments d ON s.dept_id = d.dept_id;
```

### Aggregate Functions
```sql
SELECT dept_id,
       COUNT(*)    AS total_students,
       AVG(age)    AS avg_age,
       MAX(age)    AS max_age
FROM Students
GROUP BY dept_id
HAVING COUNT(*) > 5;
```

### Subqueries
```sql
SELECT name FROM Students
WHERE age = (SELECT MAX(age) FROM Students);
```

### Views
```sql
CREATE VIEW senior_students AS
SELECT * FROM Students WHERE age >= 21;
```

### Stored Procedures (MySQL example)
```sql
DELIMITER //
CREATE PROCEDURE GetStudentsByDept(IN deptId INT)
BEGIN
    SELECT * FROM Students WHERE dept_id = deptId;
END //
DELIMITER ;
```

---

## Normalization

**Normalization** is the process of organizing a database to reduce redundancy and improve data integrity.

| Normal Form | Rule |
|-------------|------|
| **1NF** | Each column contains atomic (indivisible) values; no repeating groups |
| **2NF** | 1NF + every non-key attribute is fully dependent on the primary key |
| **3NF** | 2NF + no transitive dependencies (non-key → non-key) |
| **BCNF** | Stricter version of 3NF: every determinant is a candidate key |
| **4NF** | BCNF + no multi-valued dependencies |
| **5NF** | 4NF + no join dependencies |

### Example: 1NF Violation and Fix

❌ **Not in 1NF** (repeating group):
| StudentID | Name | Courses |
|-----------|------|---------|
| 1 | Alice | Math, Science |

✅ **1NF** (atomic values):
| StudentID | Name | Course |
|-----------|------|--------|
| 1 | Alice | Math |
| 1 | Alice | Science |

---

## Transactions & ACID Properties

A **transaction** is a logical unit of work that contains one or more SQL operations.

### ACID Properties
| Property | Description |
|----------|-------------|
| **Atomicity** | All operations succeed, or none take effect |
| **Consistency** | Database moves from one valid state to another |
| **Isolation** | Concurrent transactions do not interfere with each other |
| **Durability** | Committed transactions survive system failures |

### Example
```sql
BEGIN TRANSACTION;

UPDATE Accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE Accounts SET balance = balance + 500 WHERE account_id = 2;

COMMIT;   -- or ROLLBACK if something goes wrong
```

---

## Indexing

An **index** speeds up data retrieval at the cost of additional storage and write overhead.

### Types of Indexes
- **Primary Index**: Built on the primary key (unique, ordered).
- **Secondary Index**: Built on non-primary key columns.
- **Clustered Index**: Rows are physically sorted by the index key.
- **Non-Clustered Index**: A separate structure pointing to data rows.
- **Composite Index**: Index on multiple columns.

### Creating an Index
```sql
-- Single column index
CREATE INDEX idx_name ON Students(name);

-- Composite index
CREATE INDEX idx_dept_age ON Students(dept_id, age);

-- Drop an index
DROP INDEX idx_name ON Students;
```

> **Tip**: Use `EXPLAIN` (MySQL) or `EXPLAIN ANALYZE` (PostgreSQL) to see whether an index is being used by a query.

---

## Concurrency Control

Concurrency control ensures that multiple transactions execute safely when running simultaneously.

### Common Problems
| Problem | Description |
|---------|-------------|
| **Dirty Read** | Reading uncommitted data from another transaction |
| **Non-Repeatable Read** | Reading different values for the same row within one transaction |
| **Phantom Read** | New rows appear/disappear between reads in the same transaction |

### Isolation Levels
| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|-----------|---------------------|--------------|
| Read Uncommitted | ✅ Possible | ✅ Possible | ✅ Possible |
| Read Committed | ❌ Prevented | ✅ Possible | ✅ Possible |
| Repeatable Read | ❌ Prevented | ❌ Prevented | ✅ Possible |
| Serializable | ❌ Prevented | ❌ Prevented | ❌ Prevented |

### Locking
- **Shared Lock (S)**: Multiple transactions can read; no writes allowed.
- **Exclusive Lock (X)**: Only one transaction can read/write; others are blocked.

---

## Database Security

- **Authentication** – Verify the identity of users connecting to the database.
- **Authorization / Privileges** – Control what each user can do.
- **SQL Injection Prevention** – Always use parameterized queries or prepared statements.
- **Encryption** – Encrypt data at rest and in transit (TLS/SSL).
- **Auditing** – Log database activity for compliance and forensics.

### Granting / Revoking Permissions
```sql
-- Grant SELECT and INSERT on Students to a user
GRANT SELECT, INSERT ON Students TO 'username'@'localhost';

-- Revoke INSERT
REVOKE INSERT ON Students FROM 'username'@'localhost';
```

---

## NoSQL Databases

**NoSQL** databases are designed for large-scale, distributed data storage where flexibility and horizontal scalability are important.

### Types of NoSQL Databases
| Type | Description | Example |
|------|-------------|---------|
| **Document** | Stores JSON/BSON documents | MongoDB, CouchDB |
| **Key-Value** | Simple key → value pairs | Redis, DynamoDB |
| **Column-Family** | Data stored in column families | Cassandra, HBase |
| **Graph** | Nodes and edges representing relationships | Neo4j, ArangoDB |

### When to Use NoSQL
- Schema-less or rapidly changing data structures
- Horizontal scalability across many servers
- High-velocity reads/writes (e.g., real-time analytics)
- Storing large volumes of unstructured data

---

## Contributing

Contributions, improvements, and corrections are welcome!

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-topic`
3. Commit your changes: `git commit -m "Add notes on <topic>"`
4. Push to your branch: `git push origin feature/your-topic`
5. Open a Pull Request

Please ensure any additions are accurate, well-formatted, and consistent with the existing style.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

> **Happy Learning! 🎓** If you found this useful, please ⭐ the repository.
