# Creating Tables in Database Design

This module covers the **table creation process** as part of overall database design, focusing on how to define table structures in SQL (particularly PostgreSQL). It also shows the use of **Data Definition Language (DDL)** commands such as `CREATE`, `ALTER`, and `DROP`.

---

## 1. Database Design Stages

When designing a database, the following steps are generally followed:

1. **Mission Statement**: Define the database’s primary purpose.  
2. **Create Table Structure**: Plan the tables required based on the system's data needs.  
3. **Determine Table Relationships**: Identify how tables connect via primary keys (PK) and foreign keys (FK).  
4. **Review Data Integrity**: Ensure that constraints (e.g., `NOT NULL`, `CHECK`, `UNIQUE`) are in place.  
5. **Determine Views**: Create virtual tables to simplify frequent or complex queries.  
6. **Determine Business Rules**: Incorporate company rules (e.g., mandatory relationships or valid ranges) into the database.

This module focuses on **Step #2: Create Table Structure**.

---

## 2. Create Table Structure

1. **Identify the Required Entities (Tables)**  
   - Pinpoint the main objects in the system that will each correspond to a table (e.g., `product`, `customer`).

2. **Describe the Function of Each Table**  
   - Determine what data each table will store (e.g., `product` holds product info like price, stock).

3. **List the Fields (Columns) Needed for Each Table**  
   - For example, `product` might have `product_id`, `name`, `price`, etc.

4. **Determine Keys**  
   - **Primary Key**: Uniquely identifies each record in the table.  
   - **Foreign Key**: Links this table to another table’s primary key.

5. **Apply the Table Design to the Database**  
   - Use SQL commands (DDL) to implement the final structure (e.g., `CREATE TABLE` statements).

---

## 3. Introduction to SQL Commands

### 3.1. Data Definition Language (DDL)

- **Definition**: DDL commands define or modify the structure of database objects (tables, indexes, views, etc.).
- **Common DDL Commands**:
  1. `CREATE`: Creates new database objects (tables, schemas, etc.).
  2. `ALTER`: Modifies existing objects (e.g., adding/removing columns).
  3. `DROP`: Removes existing objects from the database.

> **Example**:  
> ```sql
> SELECT * FROM employee;
> ```
> This *queries* the data (DML), but here we focus on DDL for creating tables.

---

## 4. Creating Tables: Example with “Zolara” E-commerce

Assume an **e-commerce scenario** called **Zolara**, which has various tables (e.g., `product`, `product_category`, `product_brand`).

### 4.1. Basic Table Creation

Consider the `product` table in PostgreSQL:

```sql
CREATE TABLE product(
    product_id INTEGER PRIMARY KEY,
    product_category_id INTEGER,
    product_brand_id INTEGER,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT,
    price NUMERIC,
    stock NUMERIC,
    image BYTEA
);

Explanation:

product_id INTEGER PRIMARY KEY:
Declares product_id as the primary key (uniquely identifies each product).
name VARCHAR(255) UNIQUE NOT NULL:
UNIQUE ensures no two products share the same name.
NOT NULL forces every product to have a name.
Other columns (description, price, stock, image) don’t have constraints here but can be added if needed.
### 4.2. Viewing Table Metadata
In PostgreSQL, to see the table’s column info:

```sql
SELECT table_name,
       column_name,
       data_type,
       is_nullable
FROM information_schema.columns
WHERE table_name = 'product';
```
## 5. Auto-Increment Fields
In PostgreSQL, using SERIAL (or BIGSERIAL) provides an auto-increment integer column. For example:

```sql
CREATE TABLE product_category(
    product_category_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT
);
```
## 6. Using Default Values
Columns can have a default value if no explicit value is inserted:

```sql
CREATE TABLE product_brand(
    product_brand_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description'
);

```
## 7. Syntax for Creating Tables (General)
A typical CREATE TABLE statement:

```sql
CREATE TABLE table_name(
    column_name data_type [DEFAULT default_value] [CONSTRAINT ...],
    ...
);
```
Common constraints include:

NOT NULL
UNIQUE
CHECK
PRIMARY KEY
FOREIGN KEY
Example usage within a single column:

```sql
CREATE TABLE example(
    example_id SERIAL PRIMARY KEY,
    example_value VARCHAR(100) NOT NULL UNIQUE,
    notes TEXT DEFAULT 'N/A'
);
```
## 8. Summary
Creating Tables is a core part of database design:
We define columns, data types, constraints, and keys.
DDL Commands (CREATE, ALTER, DROP) manage the structural aspects of the database.
Auto-increment (SERIAL) and Default Values streamline data insertion.
Ensuring primary keys and the right constraints are in place preserves data integrity.
With these examples and guidelines, you can confidently translate your table design into an actual SQL schema, forming the foundation of your database.