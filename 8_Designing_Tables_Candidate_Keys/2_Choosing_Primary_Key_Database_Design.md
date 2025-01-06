# Choosing Primary Key in Database Design

This module explains how to identify and select **primary keys** for each table, focusing on the concept of **candidate keys** (CK), **composite candidate keys** (CCK), and **alternate keys** (AK). We use a case study from **Zolara**, a retail company aiming to build an online fashion store, to illustrate the process.

---

## 1. Why Do We Need a Primary Key?

- **Identifies** each record in a table uniquely.  
- **Prevents redundancy** of data.  
- Serves as the **connector** (foreign key reference) between related tables.

### Key Characteristics

1. A **table** has exactly **one** primary key.  
2. It **exclusively identifies** a record (uniqueness).  
3. It **cannot** be null.  
4. Its value **does not change often** (stability).

---

## 2. Candidate vs Primary Key

- **Candidate Key (CK)**: Any field or combination of fields that *could* serve as a primary key.  
- **Primary Key (PK)**: The one key actually chosen to be the unique identifier.  
- **Alternate Key (AK)**: A candidate key that was **not** selected as the primary key.  
- **Composite Candidate Key (CCK)**: A candidate key consisting of **multiple fields**.  
- **Composite Alternate Key (CAK)**: A composite candidate key that is not chosen as the primary key.

### Example

If `employee_id` and `ssn` both satisfy uniqueness and non-null conditions (both are candidate keys), and `employee_id` is chosen as the primary key, then:
- `employee_id` → **Primary Key (PK)**  
- `ssn` → **Alternate Key (AK)**  

---

## 3. Case: Zolara Database

**Scenario**: “Zolara is a company building an online retail website offering various fashion products from different brands.”

### Tables in Zolara Database

1. **product**  
2. **product_category**  
3. **product_brand**  
4. **customer**  
5. **customer_address**  
6. **customer_payment**  
7. **order**  
8. **order_detail**  
9. **order_payment**  
10. **order_shipment**

We will identify **candidate keys** (CK) for each table and then select the **primary key** (PK).

---

## 4. Identify Candidate Keys

Below is a summary of the fields in each table, highlighting which ones are candidate keys (**CK**).

### 4.1. `product`

| Field               | Notes                   |
|---------------------|-------------------------|
| product_id          | **CK**                 |
| product_category_id | (int) Could be AK or FK|
| product_brand_id    | (int) Could be AK or FK|
| name               | varchar                |
| description        | text                   |
| price              | numeric                |
| stock              | numeric                |
| image              | bytea                  |

> Explanation:  
> - `product_id` is a candidate key because it can uniquely identify a product.  
> - `product_category_id` and `product_brand_id` might be unique in certain contexts, but typically they serve as **foreign keys** referencing `product_category` and `product_brand`.  

### 4.2. `product_category`

| Field               | Notes         |
|---------------------|--------------|
| product_category_id | **CK**       |
| name               | varchar      |
| description        | text         |

> **product_category_id** can serve as a candidate key because it’s a numerical identifier for each category.

### 4.3. `product_brand`

| Field          | Notes         |
|----------------|--------------|
| product_brand_id | **CK**     |
| name           | varchar      |
| description    | text         |

> **product_brand_id** can serve as a candidate key for each brand entry.

### 4.4. `customer`

| Field       | Notes                          |
|-------------|--------------------------------|
| customer_id | **CK**                         |
| username    | could be unique → AK           |
| first_name  | can repeat → part of CCK?      |
| last_name   | can repeat → part of CCK?      |
| email       | could be unique → AK           |
| phone       | can repeat, can be null        |

> **customer_id** is a strong candidate key (integer-based).  
> `username` and `email` might also be unique, so they could serve as alternate keys (AK).

### 4.5. `customer_address`

| Field           | Notes                      |
|-----------------|----------------------------|
| cust_address_id | **CK**                    |
| customer_id     | int (FK)                  |
| address         | text                      |
| province        | varchar                   |
| city            | varchar                   |
| district        | varchar                   |
| postal_code     | varchar                   |
| phone           | varchar (could be null)   |

> **cust_address_id** can be the candidate key.

### 4.6. `customer_payment`

| Field          | Notes           |
|---------------|-----------------|
| cust_payment_id | **CK**        |
| customer_id    | int (FK)       |
| payment_type   | varchar        |
| provider       | varchar        |
| account_no     | varchar        |
| expire_date    | date           |

> **cust_payment_id** is the candidate key for each payment method record.

### 4.7. `order`

| Field        | Notes     |
|--------------|----------|
| order_id     | **CK**    |
| customer_id  | int (FK) |
| order_status | varchar(100) |
| created      | timestamp    |

> **order_id** is a candidate key for each unique order.

### 4.8. `order_detail`

| Field          | Notes            |
|---------------|------------------|
| order_detail_id | **CK**         |
| order_id      | int (FK)         |
| product_id    | int (FK)         |
| quantity      | numeric          |
| total_price   | numeric          |

> **order_detail_id** can uniquely identify each row in the order detail table.

### 4.9. `order_payment`

| Field          | Notes               |
|---------------|---------------------|
| order_payment_id | **CK**           |
| order_id      | int (FK)           |
| payment_amount| numeric            |
| provider      | varchar            |
| account_number| varchar            |
| payment_status| varchar(100)       |
| payment_date  | timestamp          |
| expire_date   | timestamp          |

> **order_payment_id** is the candidate key for payment records linked to an order.

### 4.10. `order_shipment`

| Field             | Notes              |
|-------------------|--------------------|
| order_shipment_id | **CK**            |
| order_id          | int (FK)          |
| tracking_number   | varchar           |
| provider          | varchar           |
| shipment_date     | timestamp         |
| shipment_status   | varchar(100)      |

> **order_shipment_id** is the candidate key for shipment records linked to an order.

---

## 5. Selecting Primary Keys

From each table’s **candidate keys**, we choose **one** to be the **primary key** (PK).

### 5.1. `product`

| Field                | Decision   | Reason                                                |
|----------------------|-----------|-------------------------------------------------------|
| product_id           | **PK**     | Numeric field, efficient as an identifier.           |
| product_category_id  | AK (FK)    | Typically a reference to product_category.           |
| product_brand_id     | AK (FK)    | Typically a reference to product_brand.              |

**Primary Key**: `product_id`

### 5.2. `product_category`

| Field               | Decision |
|---------------------|----------|
| product_category_id | **PK**   |
| name                | -        |
| description         | -        |

**Primary Key**: `product_category_id`  
(*Numeric ID is preferred for efficient lookups.*)

### 5.3. `product_brand`

| Field         | Decision |
|---------------|----------|
| product_brand_id | **PK**  |
| name          | -        |
| description   | -        |

**Primary Key**: `product_brand_id`

### 5.4. `customer`

| Field       | Decision   | Reason                                                                 |
|-------------|-----------|-------------------------------------------------------------------------|
| customer_id | **PK**     | Numeric ID, stable, efficient.                                         |
| username    | AK         | Could be unique, but not selected as PK.                               |
| email       | AK         | Could be unique, but not selected as PK.                               |
| first_name + last_name (CCK) | CAK1  | Possibly composite, but not guaranteed unique in real world. |

**Primary Key**: `customer_id`

### 5.5. `customer_address`

| Field           | Decision |
|-----------------|----------|
| cust_address_id | **PK**   |
| customer_id     | FK       |
| address         | -        |

**Primary Key**: `cust_address_id`

### 5.6. `customer_payment`

| Field          | Decision |
|---------------|----------|
| cust_payment_id | **PK**   |
| customer_id    | FK       |

**Primary Key**: `cust_payment_id`

### 5.7. `order`

| Field       | Decision |
|-------------|----------|
| order_id    | **PK**   |
| customer_id | FK       |

**Primary Key**: `order_id`

### 5.8. `order_detail`

| Field          | Decision |
|---------------|----------|
| order_detail_id | **PK**   |
| order_id      | FK       |
| product_id    | FK       |

**Primary Key**: `order_detail_id`

### 5.9. `order_payment`

| Field          | Decision |
|---------------|----------|
| order_payment_id | **PK**   |
| order_id      | FK       |

**Primary Key**: `order_payment_id`

### 5.10. `order_shipment`

| Field             | Decision |
|-------------------|----------|
| order_shipment_id | **PK**   |
| order_id          | FK       |

**Primary Key**: `order_shipment_id`

---

## 6. Full Table Structures (with Chosen Keys)

Below are the resulting schemas, with the **primary key** (PK) indicated. Some fields (e.g., foreign keys) are also noted.

### 6.1. Product Tables

**`product`**  
- **product_id** (PK)  
- product_category_id (FK to `product_category.product_category_id`)  
- product_brand_id (FK to `product_brand.product_brand_id`)  
- name  
- description  
- price  
- stock  
- image  

**`product_category`**  
- **product_category_id** (PK)  
- name  
- description  

**`product_brand`**  
- **product_brand_id** (PK)  
- name  
- description  

### 6.2. Customer Tables

**`customer`**  
- **customer_id** (PK)  
- username (AK)  
- first_name (CAK1)  
- last_name (CAK1)  
- email (AK)  
- phone  

**`customer_address`**  
- **cust_address_id** (PK)  
- customer_id (FK)  
- address  
- province  
- city  
- district  
- postal_code  
- phone  

**`customer_payment`**  
- **cust_payment_id** (PK)  
- customer_id (FK)  
- payment_type  
- provider  
- account_no  
- expire_date  

### 6.3. Order Tables

**`order`**  
- **order_id** (PK)  
- customer_id (FK)  
- order_status  
- created  

**`order_detail`**  
- **order_detail_id** (PK)  
- order_id (FK)  
- product_id (FK)  
- quantity  
- total_price  

**`order_payment`**  
- **order_payment_id** (PK)  
- order_id (FK)  
- payment_amount  
- provider  
- account_number  
- payment_status  
- payment_date  
- expire_date  

**`order_shipment`**  
- **order_shipment_id** (PK)  
- order_id (FK)  
- tracking_number  
- provider  
- shipment_date  
- shipment_status  

---

## 7. Summary

1. **Primary Key** selection is crucial for ensuring data integrity and ease of referencing across tables.  
2. **Candidate Keys** are all possible fields (or combinations) that can serve as a primary key—choose one to become the actual PK.  
3. Fields not chosen as PK but still unique become **Alternate Keys** (AK).  
4. Composite Candidate Keys (CCK) might exist if single fields don’t suffice, though in many cases an integer PK (e.g., `*_id`) is simpler and more efficient.  
5. **Zolara** uses numeric IDs for each table’s PK, ensuring fast lookups, minimal duplication, and stable relationships.

With this design, each table is easily referenced by foreign keys, and duplication or null constraints are properly managed. This sets the foundation for a reliable, scalable database for Zolara’s retail operations.
