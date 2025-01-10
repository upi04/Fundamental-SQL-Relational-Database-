# Establishing Business Rule and Constraints

## 1. Introduction

Establishing business rules involves creating and implementing specific guidelines to manage the data in a database. These rules ensure data integrity, consistency, and proper relationships between tables.

---

## 2. Steps to Establish Business Rules

### 2.1 Select the Table
Identify the table for which the business rules will be applied. This includes analyzing the table structure and relationships with other tables.

### 2.2 Define Constraints for Each Field
Analyze each field in the table and define applicable constraints, such as:
- `NOT NULL`
- `CHECK`
- `FOREIGN KEY`

### 2.3 Test Constraints
Test constraints through various operations such as:
- Insert
- Update
- Delete

### 2.4 Document Business Rules
Clearly document:
- The business rule
- The type of rule (e.g., field-specific or relationship-specific)
- Related table structure
- The test plan

---

## 3. Case: Table `Product`

### Business Rules:
1. All fields except `description` and `image` cannot contain `NULL` values.
2. The `price` and `stock` fields must have values greater than or equal to 0.
3. The relationship with `product_category` and `product_brand` tables is **mandatory**.

---

## 4. Adding Constraints

### Create Table with Constraints
Below is the SQL syntax to create the `product` table with the defined constraints:
```sql
CREATE TABLE product(
    product_id INTEGER PRIMARY KEY,
    product_category_id INTEGER NOT NULL,
    product_brand_id INTEGER NOT NULL,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT,
    price NUMERIC NOT NULL CHECK (price >= 0),
    stock NUMERIC NOT NULL CHECK (stock >= 0),
    image BYTEA,
    CONSTRAINT fk_product_category
        FOREIGN KEY (product_category_id)
        REFERENCES product_category (product_category_id)
        ON DELETE RESTRICT,
    CONSTRAINT fk_product_brand
        FOREIGN KEY (product_brand_id)
        REFERENCES product_brand (product_brand_id)
        ON DELETE RESTRICT
);
```
Explanation:
NOT NULL Constraints:
Applied to product_category_id, product_brand_id, price, and stock.
CHECK Constraints:
Ensures price and stock values are greater than or equal to 0.
FOREIGN KEY Constraints:
Enforces relationships with product_category and product_brand.
Uses ON DELETE RESTRICT to prevent deletion of parent records if child records exist.
## 5. Adding Constraints to an Existing Table
If the product table already exists, constraints can be added using ALTER TABLE.

Add CHECK Constraint:
```sql

ALTER TABLE product
ADD CONSTRAINT chk_price CHECK (price >= 0);

ALTER TABLE product
ADD CONSTRAINT chk_stock CHECK (stock >= 0);
```
Combine CHECK Constraints:
```sql

ALTER TABLE product
ADD CONSTRAINT price_stock_check
CHECK (price >= 0 AND stock >= 0);
```
## 6. Test Plan
### 6.1 Test NOT NULL Constraint:
```sql

-- Attempt to insert a record with NULL value in `price`
INSERT INTO product (product_id, product_category_id, product_brand_id, name, price, stock)
VALUES (1, 1, 1, 'Sample Product', NULL, 10);
```
-- Expected Result: Error
###6.2 Test CHECK Constraint:
```sql

-- Attempt to insert a record with a negative `price`
INSERT INTO product (product_id, product_category_id, product_brand_id, name, price, stock)
VALUES (2, 1, 1, 'Sample Product', -5, 10);
```
-- Expected Result: Error
###6.3 Test FOREIGN KEY Constraint:
sql

-- Attempt to delete a record from `product_category` that is referenced in `product`
DELETE FROM product_category WHERE product_category_id = 1;
```
-- Expected Result: Error
## 7. Documentation
### 7.1 Business Rules:
Fields product_category_id, product_brand_id, price, and stock cannot be NULL.
price and stock must have values greater than or equal to 0.
The relationships with product_category and product_brand tables are mandatory.
### 7.2 Type of Business Rule:
Field-Specific: Constraints on price, stock, product_category_id, and product_brand_id.
Relationship-Specific: Mandatory relationships with product_category and product_brand.
### 7.3 Related Table Structure:
```sql

-- Table: product_category
CREATE TABLE product_category(
    product_category_id INTEGER PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- Table: product_brand
CREATE TABLE product_brand(
    product_brand_id INTEGER PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```
### 7.4 Test Plan:
Insert operations to validate NOT NULL and CHECK constraints.
Delete operations to validate FOREIGN KEY constraints.
## 8. Conclusion
This module provides a comprehensive guide to defining and implementing business rules and constraints for the product table. Proper testing and documentation ensure the database operates efficiently and maintains data integrity.
