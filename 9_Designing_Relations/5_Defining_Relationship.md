# Module: Defining and Adding Relationships in Database

## 1. Introduction

Establishing relationships in a database is essential for linking data between tables. Relationships are defined using **Primary Keys (PK)** and **Foreign Keys (FK)**, which ensure data integrity and facilitate efficient data management.

This module demonstrates how to:
- Define relationships in a database.
- Add relationships between tables.
- Understand parameters like `ON DELETE` and `ON UPDATE`.

---

## 2. Data Definition Language (DDL)

**DDL** commands are used to create and modify the structure of tables in a database. Relationships between tables can be established during table creation or by modifying existing tables.

---

## 3. Defining Relationships During Table Creation

### 3.1. Creating Parent Tables
Before defining relationships, create the parent tables:

#### **Example: `product_category` and `product_brand` Tables**
```sql
CREATE TABLE product_category (
    product_category_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description'
);

CREATE TABLE product_brand (
    product_brand_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description'
);
### 3.2. Creating Child Table with Relationships
Example: product Table
The product table has foreign keys referencing product_category and product_brand.

```sql

CREATE TABLE product (
    product_id SERIAL PRIMARY KEY,
    product_category_id INT NOT NULL,
    product_brand_id INT NOT NULL,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description',
    price NUMERIC NOT NULL,
    stock NUMERIC DEFAULT 0,
    image BYTEA,
    CONSTRAINT fk_product_category FOREIGN KEY (product_category_id)
        REFERENCES product_category (product_category_id),
    CONSTRAINT fk_product_brand FOREIGN KEY (product_brand_id)
        REFERENCES product_brand (product_brand_id)
);
```
## 4. Adding Relationships to Existing Tables
If the tables are already created, relationships can be added using the ALTER TABLE command.

### 4.1. Adding a Foreign Key
Syntax:
```sql

ALTER TABLE child_table
ADD CONSTRAINT constraint_name
FOREIGN KEY (fk_column)
REFERENCES parent_table (parent_column);
```
Example: Adding Relationships for product Table
```sql

ALTER TABLE product
ADD CONSTRAINT fk_product_category
FOREIGN KEY (product_category_id)
REFERENCES product_category (product_category_id);

ALTER TABLE product
ADD CONSTRAINT fk_product_brand
FOREIGN KEY (product_brand_id)
REFERENCES product_brand (product_brand_id);
```
## 5. Relationship Parameters: ON DELETE and ON UPDATE
The behavior of foreign key constraints can be modified using the ON DELETE and ON UPDATE parameters.

Parameter Actions:
Action	Description
RESTRICT	Prevents deletion or update of parent rows if child rows exist (default behavior).
NO ACTION	Similar to RESTRICT, ensures no action is taken if child rows exist.
CASCADE	Automatically deletes or updates child rows when parent rows are deleted or updated.
SET NULL	Sets the foreign key value in child rows to NULL when parent rows are deleted or updated.
SET DEFAULT	Sets the foreign key value in child rows to a default value when parent rows are deleted/updated.
Example with CASCADE:
```sql

ALTER TABLE product
ADD CONSTRAINT fk_product_category
FOREIGN KEY (product_category_id)
REFERENCES product_category (product_category_id)
ON DELETE CASCADE;
```
## 6. Visualizing Relationships with ERD
Generating ERD in pgAdmin 4:
Right-click the database.
Select "Generate ERD".
View the relationships between tables in a visual format.
## 7. Summary
Steps to Define Relationships:
Create parent tables.
Define foreign keys in child tables during creation or with ALTER TABLE.
Use ON DELETE and ON UPDATE to customize relationship behaviors.
Example Syntax:
```sql

CREATE TABLE table_name (
    field_name data_type DEFAULT value CONSTRAINT,
    CONSTRAINT constraint_name
    FOREIGN KEY (fk_columns)
    REFERENCES parent_table (parent_columns)
    [ON DELETE action]
    [ON UPDATE action]
);
```
8. Example: Complete SQL Script for Relationships
```sql

-- Creating Parent Tables
CREATE TABLE product_category (
    product_category_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description'
);

CREATE TABLE product_brand (
    product_brand_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description'
);

-- Creating Child Table with Relationships
CREATE TABLE product (
    product_id SERIAL PRIMARY KEY,
    product_category_id INT NOT NULL,
    product_brand_id INT NOT NULL,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT DEFAULT 'No description',
    price NUMERIC NOT NULL,
    stock NUMERIC DEFAULT 0,
    image BYTEA,
    CONSTRAINT fk_product_category FOREIGN KEY (product_category_id)
        REFERENCES product_category (product_category_id)
        ON DELETE CASCADE,
    CONSTRAINT fk_product_brand FOREIGN KEY (product_brand_id)
        REFERENCES product_brand (product_brand_id)
        ON DELETE CASCADE
);
```






