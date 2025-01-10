# Business Rule and Constraint Module

## 1. Introduction

This module explains the concept of **Business Rules** and their implementation using **Constraints** in database design. Business rules are critical for ensuring data integrity, enforcing organizational policies, and managing relationships between data entities.

---

## 2. What are Business Rules?

### Definition:
**Business Rules** are guidelines established by an organization to govern its operations and data management. These rules influence:
- **Data Storage**: Determines what data needs to be stored.
- **Table Structure**: Defines how tables and relationships are constructed.
- **Information Extraction**: Specifies what information can be retrieved from the database.
- **Data Security**: Ensures stored data remains secure and consistent.

### Key Considerations:
- Business rules may differ across organizations or even departments.
- Similar rules may serve different objectives depending on context.

---

## 3. Types of Business Rules

### 3.1 Database-Oriented Business Rules
- Implemented directly within the database using **Constraints**.
- Example: Limiting `kode_pos` (postal code) to exactly 9 characters.

### 3.2 Application-Oriented Business Rules
- Enforced outside the database, such as in web application or backend logic.
- Example: Allowing only admin users to access a specific dataset.

---

## 4. Constraints in Database Design

**Constraints** enforce business rules directly within the database by limiting or validating data. Below are common constraints:

| **Constraint**  | **Explanation**                                                                 |
|------------------|---------------------------------------------------------------------------------|
| **NOT NULL**     | Ensures a column cannot have `NULL` values.                                    |
| **UNIQUE**       | Prevents duplicate values in a column.                                         |
| **PRIMARY KEY**  | Combines `NOT NULL` and `UNIQUE` to uniquely identify each record.             |
| **FOREIGN KEY**  | Links columns between tables to enforce relationships.                        |
| **CHECK**        | Ensures column values meet specified conditions.                              |

---

## 5. Type of Participation

The **Type of Participation** defines whether data in related tables must exist for a relationship to be valid.

### 5.1 Mandatory
- A child record **must** be associated with a parent record.
- Parameters: `NO ACTION`, `RESTRICT`, or `CASCADE`.

### 5.2 Optional
- A child record **does not need** to be associated with a parent record.
- Parameters: `SET NULL` or `SET DEFAULT`.

---

## 6. Examples of Participation Types

### 6.1 Mandatory-Mandatory Relationship

#### Case:
A customer **must** provide their address, and an address **must** belong to a customer.

#### Database Structure:
**Customer Table**:
```sql
CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    username VARCHAR(255),
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    email VARCHAR(255),
    phone VARCHAR(20)
);
```
Customer_Address Table:

``sql

CREATE TABLE customer_address (
    cust_address_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    address TEXT,
    province VARCHAR(255),
    city VARCHAR(255),
    district VARCHAR(255),
    postal_code VARCHAR(10),
    phone VARCHAR(20),
    CONSTRAINT fk_customer FOREIGN KEY (customer_id)
        REFERENCES customer (customer_id)
        ON DELETE RESTRICT
);
```
Diagram:
The outer vertical line denotes the mandatory relationship between customer and customer_address.
### 6.2 Mandatory-Optional Relationship
Case:
A customer may provide payment information, but it is not mandatory.

Database Structure:
Customer Table:

```sql

CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    username VARCHAR(255),
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    email VARCHAR(255),
    phone VARCHAR(20)
);
```
Customer_Payment Table:

```sql

CREATE TABLE customer_payment (
    cust_payment_id INT PRIMARY KEY,
    customer_id INT,
    payment_type VARCHAR(255),
    provider VARCHAR(255),
    account_no VARCHAR(255),
    expire_date DATE,
    CONSTRAINT fk_customer FOREIGN KEY (customer_id)
        REFERENCES customer (customer_id)
        ON DELETE SET NULL
);
```
Diagram:
The outer circle represents the optional relationship between customer and customer_payment.
## 7. Testing Business Rules
Example: Table Product
Business Rules:
Fields product_category_id, product_brand_id, price, and stock must not contain NULL values.
Fields price and stock must be greater than or equal to 0.
Relationships with product_category and product_brand are mandatory.
SQL Implementation:
```sql

CREATE TABLE product (
    product_id INT PRIMARY KEY,
    product_category_id INT NOT NULL,
    product_brand_id INT NOT NULL,
    name VARCHAR(255) UNIQUE NOT NULL,
    description TEXT,
    price NUMERIC NOT NULL CHECK (price >= 0),
    stock NUMERIC NOT NULL CHECK (stock >= 0),
    image BYTEA,
    CONSTRAINT fk_product_category FOREIGN KEY (product_category_id)
        REFERENCES product_category (product_category_id)
        ON DELETE RESTRICT,
    CONSTRAINT fk_product_brand FOREIGN KEY (product_brand_id)
        REFERENCES product_brand (product_brand_id)
        ON DELETE RESTRICT
);
```
8. Conclusion
Defining business rules and constraints is essential for ensuring the integrity, consistency, and functionality of a database. Properly implemented rules reduce redundancy, enforce relationships, and improve data security.

This module provides a comprehensive guide to implementing and testing business rules and constraints, offering clarity and efficiency in database management.



