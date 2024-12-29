# **Database & SQL**

## **What is a Database?**
A database is an organized collection of data stored and accessed electronically. Databases are designed to manage large amounts of information efficiently and enable easy retrieval, manipulation, and storage of data.

### **Examples of Database Systems**
- **Relational Databases**: MySQL, PostgreSQL, SQLite, Microsoft SQL Server, Oracle Database.
- **Non-Relational Databases (NoSQL)**: MongoDB, Cassandra, Redis.

---

## **What is SQL?**
SQL (Structured Query Language) is the standard language used to interact with relational databases. It allows users to query, manipulate, and define data effectively.

### **Key Functions of SQL**
1. **Querying Data**: Retrieving specific information from one or more tables.
   ```sql
   SELECT column1, column2
   FROM table_name;
   ```
2. **Manipulating Data**:
   - Insert new records: 
     ```sql
     INSERT INTO table_name (column1, column2) VALUES (value1, value2);
     ```
   - Update existing records:
     ```sql
     UPDATE table_name
     SET column1 = value1
     WHERE condition;
     ```
   - Delete records:
     ```sql
     DELETE FROM table_name WHERE condition;
     ```
3. **Defining Database Structure**:
   - Create tables:
     ```sql
     CREATE TABLE table_name (
         column1 datatype,
         column2 datatype,
         ...
     );
     ```
   - Modify table structure:
     ```sql
     ALTER TABLE table_name ADD column_name datatype;
     ```
   - Drop tables:
     ```sql
     DROP TABLE table_name;
     ```

---

## **Common Data Types in Databases**

### **String Data Types**
- **CHAR**: Fixed-length character strings (e.g., `CHAR(10)`).
- **VARCHAR**: Variable-length character strings (e.g., `VARCHAR(255)`).
- **TEXT**: Large text fields.

### **Numeric Data Types**
- **INT**: Integer values.
- **FLOAT/DOUBLE**: Floating-point numbers.
- **DECIMAL (NUMERIC)**: Fixed-point numbers for precise values (e.g., monetary data).

### **Date/Time Data Types**
- **DATE**: Stores dates (e.g., `YYYY-MM-DD`).
- **DATETIME**: Stores date and time (e.g., `YYYY-MM-DD HH:MM:SS`).
- **TIMESTAMP**: Stores date and time with timezone adjustments.

---

SQL is an essential tool for working with relational databases, enabling efficient management and analysis of data. With SQL, you can interact with structured data effectively, perform complex queries, and design robust data-driven applications.
