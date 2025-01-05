# Case Study: Database Design for Zolara Online Retail

Zolara is a retail website offering a wide range of fashion products from various brands. This case study outlines how to design a database that supports product management, customer management, and shop management (orders, payments, and shipments).

---

## 1. Mission Statement

The **database** must handle:

1. Products separated by **category** and **brand**.
2. **Orders** from customers, including payment and shipment details.

> **Example Mission**:  
> “Our Zolara database is intended to store and manage all product information, customer data, orders (payments, shipments), and ensure the website can function smoothly for retail operations.”

---

## 2. Creating Table Structures

We break down the main subjects or entities needed to fulfill the mission:

### 2.1. Entity Breakdown

- **product**  
- **product_category**  
- **product_brand**

- **customer**  
- **order**  
- **payment**  
- **shipment**

### 2.2. Tables and Their Descriptions

| Table              | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| `product`          | Stores product details (items in the store)                            |
| `product_category` | Stores product categories                                              |
| `product_brand`    | Stores product brand information                                       |
| `customer`         | Stores customer details                                                |
| `order`            | Stores information about product orders placed by customers            |
| `order_detail`     | Stores details of each product within an order (quantities, prices)    |
| `payment`          | Stores payment information for an order                                |
| `shipment`         | Stores shipment details for orders that have been paid                 |

*(In some diagrams, these may appear slightly differently named, such as `order_payment` or `order_shipment`, etc.)*

---

## 3. Database Diagrams

### 3.1. Product Management
+-----------------+            +-------------------+           +-------------------+
|   product       |            | product_category  |           | product_brand     |
+-----------------+            +-------------------+           +-------------------+
| product_id      |            | product_category_id|           | product_brand_id  |
| product_name    |            | category_name      |           | brand_name        |
| product_desc    |            | category_desc      |           | brand_desc        |
| product_price   |            +-------------------+           +-------------------+
| product_stock   |
| product_image   |
| product_category_id -------->|
| product_brand_id ------------>|
+-----------------+
**Explanation**:

- `product` is linked to `product_category` (via `product_category_id`) and `product_brand` (via `product_brand_id`).  
- Each product has `name`, `price`, `stock`, `image`, and a description.

### 3.2. Customer Management
+------------------+           +--------------------+
|   customer       |           |  customer_address  |
+------------------+           +--------------------+
| customer_id      |           | address_id         |
| username         |           | customer_id ------>|
| first_name       |           | address            |
| last_name        |           | province           |
| email            |           | city               |
| phone            |           | district           |
| customer_address | --------> | postal_code        |
| customer_payment | --------> | phone              |
+------------------+           +--------------------+

                      +--------------------+
                      |  customer_payment  |
                      +--------------------+
                      | cust_payment_id    |
                      | customer_id ------>|
                      | payment_type       |
                      | provider           |
                      | account_no         |
                      | expire_date        |
                      +--------------------+

**Explanation**:

- `customer` can have multiple addresses (`customer_address`) and multiple payment methods (`customer_payment`).  
- Fields like `username`, `email`, and `phone` are part of `customer` details.

### 3.3. Order and Order Detail

+------------------+           +--------------------+
|     order        |           |    order_detail    |
+------------------+           +--------------------+
| order_id         |           | order_detail_id    |
| customer_id ----->|           | order_id --------->|
| total_price      |           | product_id         |
| order_status     |           | quantity           |
| order_created    |           +--------------------+
+------------------+

**Explanation**:

- An `order` belongs to a single `customer`.
- The `order_detail` table captures each **product** in that order, allowing a one-to-many relationship (one order, many items).

### 3.4. Payment and Shipment
+--------------------+           +----------------------+
|   order_payment    |           |    order_shipment    |
+--------------------+           +----------------------+
| order_payment_id   |           | order_shipment_id    |
| order_id --------->|           | order_id ----------->|
| payment_amount     |           | tracking_number      |
| provider           |           | provider             |
| account_number     |           | shipment_date        |
| payment_status     |           | shipment_status      |
| payment_date       |           +----------------------+
| expire_date        |
+--------------------+

**Explanation**:

- **order_payment** stores payment info for an order, including status and date.  
- **order_shipment** stores shipping info like tracking number, provider, shipment date, and status.

---

## 4. Product Management

### 4.1. Entity Relationship Diagram
+------------------+          +-----------------------+
|     product      |          |   product_category    |
+------------------+          +-----------------------+
| product_id (PK)  |          | product_category_id   |
| product_category_id (FK) -->| (PK)                 |
| product_brand_id (FK) ----->| name                 |
| name            |          | description          |
| description     |          +-----------------------+
| price           |
| stock           |          +-----------------------+
| image           |          |   product_brand       |
+------------------+          +-----------------------+
                               | product_brand_id (PK)|
                               | name                |
                               | description         |
                               +-----------------------+

**Explanation**:
- **`product`** has a **many-to-one** relationship to both **`product_category`** and **`product_brand`**. 
- This means multiple products can share the same category or brand.

---


## 5. Customer Management

### 5.1. Entity Relationship Diagram

+------------------+          +-----------------------+
|    customer      |          |  customer_address     |
+------------------+          +-----------------------+
| customer_id (PK) |          | address_id (PK)       |
| username         |          | customer_id (FK) ---->|
| first_name       |          | address              |
| last_name        |          | province             |
| email            |          | city                 |
| phone            |          | district             |
+------------------+          | postal_code          |
                               | phone               |
                               +-----------------------+

+-----------------------+
|   customer_payment    |
+-----------------------+
| cust_payment_id (PK)  |
| customer_id (FK) ----->|
| payment_type          |
| provider              |
| account_number        |
| expire_date           |
+-----------------------+


**Explanation**:
- **`customer`** can have multiple **`customer_address`** records (one-to-many).
- **`customer`** can also have multiple **`customer_payment`** options (one-to-many).

---

## 6. Shop Management

### 6.1. Diagram: Order, Order Detail, and Related Entities

+------------------+          +---------------------+          +------------------+
|    customer      |          |       order         |          |   order_detail   |
+------------------+          +---------------------+          +------------------+
| customer_id (PK) |          | order_id (PK)       |          | order_detail_id  |
| username         |          | customer_id (FK) -->|          | (PK)             |
| firstName        |          | status             |          | order_id (FK) -->|
| lastName         |          | created            |          | product_id (FK) ->|
| email            |          | total_price        |          | quantity          |
| phone            |          +---------------------+          +------------------+
+------------------+                  |
                                     +---------------------+
                                     |   order_shipment    |
                                     +---------------------+
                                     | shipment_id (PK)    |
                                     | order_id (FK) ----->|
                                     | tracking_number     |
                                     | provider            |
                                     | date                |
                                     | status              |
                                     +---------------------+

+---------------------+
|    order_payment    |
+---------------------+
| payment_id (PK)     |
| order_id (FK) ----->|
| amount              |
| provider            |
| account_number      |
| payment_date        |
| expire_date         |
+---------------------+
+------------------+          +---------------------+          +------------------+
|    customer      |          |       order         |          |   order_detail   |
+------------------+          +---------------------+          +------------------+
| customer_id (PK) |          | order_id (PK)       |          | order_detail_id  |
| username         |          | customer_id (FK) -->|          | (PK)             |
| firstName        |          | status             |          | order_id (FK) -->|
| lastName         |          | created            |          | product_id (FK) ->|
| email            |          | total_price        |          | quantity          |
| phone            |          +---------------------+          +------------------+
+------------------+                  |
                                     +---------------------+
                                     |   order_shipment    |
                                     +---------------------+
                                     | shipment_id (PK)    |
                                     | order_id (FK) ----->|
                                     | tracking_number     |
                                     | provider            |
                                     | date                |
                                     | status              |
                                     +---------------------+

+---------------------+
|    order_payment    |
+---------------------+
| payment_id (PK)     |
| order_id (FK) ----->|
| amount              |
| provider            |
| account_number      |
| payment_date        |
| expire_date         |
+---------------------+
+------------------+          +--------------------+          +------------------+
|    customer      |          |  customer_address  |          | customer_payment |
+------------------+          +--------------------+          +------------------+
| customer_id (PK) |          | address_id (PK)    |          | cust_payment_id  |
| username         |          | customer_id (FK) ->|          | (PK)             |
| firstName        |          | address           |          | customer_id (FK) ->|
| lastName         |          | city              |          | payment_type      |
| email            |          | postal_code       |          | provider          |
| phone            |          +--------------------+          | account_no        |
+------------------+                                           | expire_date       |
                                                              +------------------+

+---------------------+          +---------------------+          +------------------+
|       order         |          |   order_detail      |          |   product        |
+---------------------+          +---------------------+          +------------------+
| order_id (PK)       |          | order_detail_id (PK)|          | product_id (PK)  |
| customer_id (FK) -->|          | order_id (FK) ----->|          | category_id (FK)->|
| status              |          | product_id (FK) -->|          | brand_id (FK) -->|
| created             |          | quantity           |          | name             |
| total_price         |          +---------------------+          | price            |
+---------------------+                                           +------------------+

+---------------------+          +---------------------+
|   order_payment     |          |   order_shipment    |
+---------------------+          +---------------------+
| payment_id (PK)     |          | shipment_id (PK)    |
| order_id (FK) ----->|          | order_id (FK) ----->|
| amount              |          | tracking_number     |
| provider            |          | provider            |
| account_no          |          | date                |
| expire_date         |          | status              |
+---------------------+          +---------------------+


## 7. Determining Table Relationships

We can divide them into three main areas:

1. **Product Management**: `product` ↔ `product_category` ↔ `product_brand`  
2. **Customer Management**: `customer` ↔ `customer_address` ↔ `customer_payment`  
3. **Shop Management**: `order`, `order_detail`, `order_payment`, `order_shipment`, plus the link to `customer`.

### 4.1. Business Rules (Examples)

- A **customer** must have at least one **address** before placing an order.  
- A **customer** can have zero or more **payment** methods on file.  
- A **product_category** and **product_brand** must each have at least one product.  
- For an **order** to be valid, it must link to an existing customer.  
- Payment details (order_payment) should exist only if an order is placed.

---

## 8. Determining Views

Some example **views** that might be useful:

1. **Monthly Revenue**  
   - Summation of all `order_payment` amounts or `order` totals per month.  
2. **Best-Selling Products by Month**  
   - Ranking `product_id` from `order_detail` by total quantity sold.  
3. **Popular Product Brands by Month**  
   - Summation of items sold grouped by `product_brand`.  
4. **Repeat Customer Analysis**  
   - Identifying which customers place multiple orders in a given time frame.

*These views can simplify reporting tasks for managers or analysts.*

---

## 9. Summary

### Overall Relational Structure

1. **Product Management**:  
   - `product` links to `product_category` and `product_brand`.  
2. **Customer Management**:  
   - `customer` can have multiple `customer_address` and `customer_payment`.  
3. **Shop Management**:  
   - `order` (linked to a single customer)  
   - `order_detail` (one order can have many products)  
   - `order_payment` and `order_shipment` (each order can have associated payment and shipment records).

### Key Takeaways

- **Well-defined tables**: `product`, `customer`, `order`, `payment`, `shipment`, etc.  
- **Clear primary/foreign key relationships**: e.g., `order.customer_id → customer.customer_id`.  
- **Business Rules** reflect real-world constraints.  
- **Views** provide quick reporting and reduce repetitive query writing.

This design lays the groundwork for a **scalable** and **maintainable** retail database system for **Zolara** (or similar online stores).

