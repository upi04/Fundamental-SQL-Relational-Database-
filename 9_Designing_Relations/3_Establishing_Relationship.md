# Module: Establishing Relationships in Databases

## 1. Introduction

Relationships in databases establish how data in different tables relate to one another. These relationships are built using **Foreign Keys** (FK), which reference **Primary Keys** (PK) in other tables.

---

## 2. Foreign Key

### Definition:
- A **Foreign Key (FK)** is a field in a table that uniquely identifies a row in another table.
- Ideally, the name of the FK matches the name of the PK it references for ease of design and querying.
- The FK and PK names can differ if needed, but it’s important to maintain consistency.

### Constraints:
- The values in the FK field must correspond to values in the PK field it references.
- For example:  
  Only customers present in the `customer` table can place orders.

#### Example:

**Customer Table**  
| **Column**      | **Type** | **Key** |
|------------------|----------|---------|
| customer_id      | int      | PK      |
| username         | varchar  |         |
| first_name       | varchar  |         |
| last_name        | varchar  |         |
| email            | varchar  |         |
| phone            | varchar  |         |

**Order Table**  
| **Column**      | **Type** | **Key** |
|------------------|----------|---------|
| order_id         | int      | PK      |
| customer_id      | int      | FK      |
| order_status     | varchar  |         |
| created          | timestamp|         |

---

## 3. Types of Relationships

### 3.1. One-to-One Relationship

#### Definition:
- A single row in one table is related to a single row in another table.

#### Example:
- Each **Department** has one **Supervisor**.

**Supervisor Table**  
| **Column**      | **Type** | **Key** |
|------------------|----------|---------|
| supervisor_id    | int      | PK      |
| first_name       | varchar  |         |
| last_name        | varchar  |         |
| address          | varchar  |         |

**Department Table**  
| **Column**      | **Type** | **Key** |
|------------------|----------|---------|
| department_id    | int      | PK      |
| supervisor_id    | int      | FK      |
| name             | varchar  |         |

#### Relationship:
- The `supervisor_id` in the `Supervisor` table is the PK.
- The `supervisor_id` in the `Department` table is the FK.

#### SQL Example:
```sql
CREATE TABLE Supervisor (
    supervisor_id SERIAL PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    address VARCHAR(255)
);

CREATE TABLE Department (
    department_id SERIAL PRIMARY KEY,
    supervisor_id INT REFERENCES Supervisor(supervisor_id),
    name VARCHAR(255)
);
```
### 3.2. One-to-Many Relationship
Definition:
A single row in one table relates to multiple rows in another table.
Example:
A Supplier supplies multiple Products.
Products Table

Column	Type	Key
product_id	int	PK
name	varchar	AK
price	numeric	
stock	numeric	
supplier_id	int	FK
Supplier Table

Column	Type	Key
supplier_id	int	PK
name	varchar	
address	varchar	
phone	varchar	
SQL Example:
```sql

CREATE TABLE Supplier (
    supplier_id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    address VARCHAR(255),
    phone VARCHAR(15)
);

CREATE TABLE Products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL,
    price NUMERIC NOT NULL,
    stock NUMERIC NOT NULL,
    supplier_id INT REFERENCES Supplier(supplier_id)
);
```
### 3.3. Many-to-Many Relationship
Definition:
Many rows in one table relate to many rows in another table.
Requires a linking table to establish the relationship.
Example:
A single Order contains multiple Products.
A single Product can appear in multiple Orders.
Order Table

Column	Type	Key
order_id	int	PK
customer_id	int	FK
order_date	timestamp	
Products Table

Column	Type	Key
product_id	int	PK
name	varchar	
price	numeric	
stock	numeric	
Order Details Table

Column	Type	Key
order_id	int	PK/FK
product_id	int	PK/FK
quantity_ordered	int	
quote_price	numeric	
SQL Example:
```sql

CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT,
    order_date TIMESTAMP
);

CREATE TABLE Products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    price NUMERIC,
    stock NUMERIC
);
```

CREATE TABLE Order_Details (
    order_id INT REFERENCES Orders(order_id),
    product_id INT REFERENCES Products(product_id),
    quantity_ordered INT,
    quote_price NUMERIC,
    PRIMARY KEY (order_id, product_id)
);
```