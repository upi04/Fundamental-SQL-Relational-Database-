# Modifying and Dropping Tables in Database Design

This module explains how to **modify existing tables** and **drop tables** using PostgreSQL syntax. These operations are typically part of the **Data Definition Language (DDL)** commands that allow us to change table structures after they have been created.

---

## 1. Modifying Table Structure

As a database evolves, you may need to correct mistakes or add new features. PostgreSQL provides the `ALTER TABLE` command for such modifications.

### 1.1. Adding a New Column

**Scenario**: We want to add a new column named `made_in` to the `product` table.

```sql
ALTER TABLE product
ADD COLUMN made_in VARCHAR(255);
```
ALTER TABLE product: Tells PostgreSQL that we are modifying the product table.
ADD COLUMN made_in VARCHAR(255): Adds a new column named made_in of type VARCHAR(255).
### 1.2. Viewing Table Description
To verify your new column, use the following query:

```sql
SELECT table_name,
       column_name,
       data_type,
       is_nullable
FROM information_schema.columns
WHERE table_name = 'product';
```
Output: You should see the new made_in column listed with its data type (varchar(255)).

##2. Syntax of Modifying a Table
A general pattern for modifying a table:
Where ACTION can be:

ADD COLUMN: Adds a new column to the table.
DROP COLUMN: Removes an existing column.
ADD CONSTRAINT: Adds a new constraint (e.g., CHECK, UNIQUE, PRIMARY KEY).
DROP CONSTRAINT: Removes an existing constraint from a table.
RENAME COLUMN: Renames a column in a table.
RENAME TO: Renames the table itself.
ALTER COLUMN: Changes the data type of a column or other properties

Example commands:

```sql
Copy code
-- 1) Menambah kolom
ALTER TABLE product
ADD COLUMN color VARCHAR(50);

-- 2) Menghapus kolom
ALTER TABLE product
DROP COLUMN made_in;

-- 3) Mengganti nama kolom
ALTER TABLE product
RENAME COLUMN color TO product_color;

-- 4) Mengganti nama tabel
ALTER TABLE product
RENAME TO product_new;

-- 5) Mengubah tipe data kolom
ALTER TABLE product
ALTER COLUMN price TYPE NUMERIC(10,2);
```
## 3. Dropping Tables
In some cases, you may need to completely remove a table from your database schema.

### 3.1. Example: Dropping product_brand
Scenario: An administrator wants to delete the product_brand table. Suppose this table has no active relationships to other tables.
```sql
DROP TABLE product_brand;
```
### 3.2. Syntax for Dropping Tables
```sql

DROP TABLE table_name [ CASCADE | RESTRICT ];
```
CASCADE: Automatically drops dependent objects (e.g., foreign key references in other tables).
RESTRICT: The default behavior; prevents dropping the table if there are any dependencies on it.
Important: Using CASCADE can remove multiple related objects, so use with caution.
### 4. Summary
Modifying Tables:

Use ALTER TABLE to add or drop columns, rename columns/tables, or modify column types.
Check table structure changes with information_schema.columns.
Dropping Tables:

Use DROP TABLE table_name [CASCADE | RESTRICT];.
CASCADE removes dependent objects; RESTRICT ensures the table won’t be dropped if it’s still referenced by others.
These operations give you flexibility to adapt your database structure over time, whether adding new fields or removing unused tables. Always confirm dependencies before dropping or drastically altering tables to avoid unintended data loss.