# Inserting Data with SQL (Data Manipulation Language)

This module covers how to **insert data** into tables using **SQL**, particularly focusing on **PostgreSQL** examples. We will look at different ways to insert single or multiple rows, how to handle auto-increment primary keys, and the general syntax for `INSERT` statements.

---

## 1. SQL Command Types

In SQL, there are multiple types of commands, each serving a distinct purpose:

1. **Data Query Language (DQL)**
   - Example: `SELECT` — Used to retrieve data from the database.

2. **Data Definition Language (DDL)**
   - Examples: `CREATE`, `ALTER`, `DROP` — Define or modify database structure (e.g., tables, schemas).

3. **Data Manipulation Language (DML)**
   - Examples: `INSERT`, `UPDATE`, `DELETE` — Manipulate the data *within* the tables.

In this module, we focus on **DML**, specifically the **`INSERT`** command to add new records to a table.

---

## 2. DML: INSERT, UPDATE, DELETE

### 2.1. INSERT

Used to **add** new rows to a table.

### 2.2. UPDATE

Used to **modify** existing rows in a table.

### 2.3. DELETE

Used to **remove** existing rows from a table.

---

## 3. Example: `product_category` Table

Assume a table structure:

| Column               | Data Type              | Description                                 |
|----------------------|------------------------|---------------------------------------------|
| **product_category_id** | **PK** / Integer      | Primary key (possibly auto-increment)        |
| name                 | Character Varying(255) | Name or label of the product category        |
| description          | Text                   | Additional details about the category        |

### 3.1. Sample Data for `product_category`

1, 'Shirt', 'A shirt is a piece of clothing worn...' 2, 'T-shirt', 'T-shirt has short sleeves and a round...' 3, 'Trousers', 'Trouser is an outer garment covering...' 4, 'Skirt', 'A skirt is the lower part of a dress or...' 5, 'Outerwear','Outerwear is clothing and accessories...' 6, 'Hat', 'A hat is a covering for the head usually...' 7, 'Sandal', 'Type of footwear consisting of a sole...'


We will show how to insert these records into the `product_category` table.

---

## 4. Inserting Data: Two Approaches

### 4.1. Approach 1: Without Specifying Column Names

You list **all columns** in the order they appear in the table definition.

**Example**:
```sql
INSERT INTO product_category
VALUES (1, 'Shirt', 'A shirt is a piece of clothing worn on the upper part of your body with a collar, sleeves, and buttons down the front.');
```
INSERT INTO product_category: Instructs PostgreSQL to insert into the product_category table.
VALUES (...): The parentheses must match the exact order of columns in the table definition.
Note: This method requires you to remember the exact column order in the table, which can lead to errors if the table structure changes.

### 4.2. Approach 2: Specifying Column Names
You list the specific columns you want to insert into, along with corresponding values.

Example:
```sql
INSERT INTO product_category (name, product_category_id, description)
VALUES ('Shirt', 1, 'A shirt is a piece of clothing worn on the upper part of your body with a collar, sleeves, and buttons down the front.');
```
The order in the VALUES(...) clause matches the order of the columns you wrote.
You do not need to list columns in the same order as the table structure, but your VALUES(...) must match the columns you specified.
Pros:

More flexible; if the table structure changes, you only need to adjust the code for the columns you specify.
Clearer which columns get which value
## 5. Viewing Inserted Data
Use a SELECT statement to confirm the inserted rows:

```sql

SELECT * FROM product_category;
If the insert was successful, you’ll see a row with product_category_id = 1 (for the previous example).

## 6. Auto-Increment Primary Keys (SERIAL)
If your primary key column is of type SERIAL, it auto-increments. This means you do not have to specify the ID each time you insert.

### 6.1. Inserting Without Primary Key
Example: Inserting a second record for T-shirt without specifying product_category_id:

Option 1: Use DEFAULT for the primary key column

```sql

INSERT INTO product_category(product_category_id, name, description)
VALUES (DEFAULT, 'T-shirt', 'T-shirt has short sleeves and a round neckline...');
Option 2: Simply omit the primary key column
```
```sql

INSERT INTO product_category(name, description)
VALUES ('T-shirt', 'T-shirt has short sleeves and a round neckline...');
```
Both approaches rely on product_category_id being auto-generated.

## 7. Inserting Multiple Rows at Once
You can insert multiple rows in a single statement by separating each set of values with a comma:

```sql

INSERT INTO product_category(name, description)
VALUES
  ('Trousers', 'Trouser is an outer garment covering the lower half of the body...'),
  ('Skirt', 'A skirt is the lower part of a dress or a separate outer garment...'),
  ('Outerwear', 'Outerwear is clothing and accessories worn outdoors...');
```
This example will add three new rows to product_category.
8. Checking the Final Data
After multiple inserts, check again with:

```sql

SELECT * FROM product_category;
Example Output (sample data):
```
```sql

 product_category_id |   name       | description
---------------------+--------------+-----------------------------------------
                   1 | Shirt        | A shirt is a piece of clothing worn ...
                   2 | T-shirt      | T-shirt has short sleeves ...
                   3 | Trousers     | Trouser is an outer garment ...
                   4 | Skirt        | A skirt is the lower part of a ...
                   5 | Outerwear    | Outerwear is clothing and ...
```
(5 rows)
9. General INSERT Syntax
In summary, the general form of an INSERT statement is:

```sql

INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```
You can also insert multiple rows in the same statement by adding more (value...) tuples, separated by commas.

## 10. Conclusion
INSERT is a fundamental DML operation to add records to a table.
You can specify columns explicitly (INSERT INTO table_name(col1, col2) VALUES(...)) or rely on the table’s column order.
Auto-increment primary keys (SERIAL in PostgreSQL) simplify not needing to track the next ID.
Multiple row inserts are supported in a single query, improving performance and reducing round-trips to the database.
