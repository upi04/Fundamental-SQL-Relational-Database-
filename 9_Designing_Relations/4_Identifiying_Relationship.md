# Module: Identifying Relationships and Creating ERD for Zolara Database

## 1. Introduction

The Zolara Database contains multiple tables representing different entities in the e-commerce domain. These tables are interrelated through various types of relationships:

- **1:1** → One-to-one
- **1:N** → One-to-many
- **M:N** → Many-to-many

This module explores the relationships between these tables, identifies their primary and foreign keys, and provides an **Entity Relationship Diagram (ERD)** to visualize their connections.

---

## 2. Table Overview and Relationships

### Tables in the Zolara Database:
- **product**
- **product_category**
- **product_brand**
- **customer**
- **customer_address**
- **customer_payment**
- **order**
- **order_detail**
- **order_payment**
- **order_shipment**

### Identified Relationships:
The relationships between these tables are categorized as follows:

| Parent Table       | Child Table         | Relationship |
|--------------------|---------------------|--------------|
| product_category   | product             | 1:N          |
| product_brand      | product             | 1:N          |
| customer           | customer_address    | 1:N          |
| customer           | customer_payment    | 1:N          |
| order              | customer            | 1:N          |
| order              | order_payment       | 1:1          |
| order              | order_shipment      | 1:1          |
| order_detail       | order               | 1:N          |
| order_detail       | product             | 1:N          |

---

## 3. Detailed Entity Relationship Diagram (ERD)

### 3.1. **Product and Related Tables**
#### Relationships:
- `product` → `product_category`: **1:N**
- `product` → `product_brand`: **1:N**

#### Table Structures:
**product**
- **PK**: `product_id`
- **FK**: `product_category_id`, `product_brand_id`
- Other columns: `name`, `description`, `price`, `stock`, `image`

**product_category**
- **PK**: `product_category_id`
- Other columns: `name`, `description`

**product_brand**
- **PK**: `product_brand_id`
- Other columns: `name`, `description`

---

### 3.2. **Customer and Related Tables**
#### Relationships:
- `customer` → `customer_address`: **1:N**
- `customer` → `customer_payment`: **1:N**

#### Table Structures:
**customer**
- **PK**: `customer_id`
- Other columns: `username`, `first_name`, `last_name`, `email`, `phone`

**customer_address**
- **PK**: `cust_address_id`
- **FK**: `customer_id`
- Other columns: `address`, `province`, `city`, `district`, `postal_code`, `phone`

**customer_payment**
- **PK**: `cust_payment_id`
- **FK**: `customer_id`
- Other columns: `payment_type`, `provider`, `account_no`, `expire_date`

---

### 3.3. **Order and Related Tables**
#### Relationships:
- `order_detail` → `order`: **1:N**
- `order_detail` → `product`: **1:N**
- `order` → `customer`: **1:N**
- `order` → `order_payment`: **1:1**
- `order` → `order_shipment`: **1:1**

#### Table Structures:
**order**
- **PK**: `order_id`
- **FK**: `customer_id`
- Other columns: `order_status`, `created`

**order_detail**
- **PK**: `order_detail_id`
- **FKs**: `order_id`, `product_id`
- Other columns: `quantity`, `total_price`

**order_payment**
- **PK**: `order_payment_id`
- **FK**: `order_id`
- Other columns: `payment_amount`, `payment_type`, `payment_date`, `payment_status`

**order_shipment**
- **PK**: `order_shipment_id`
- **FK**: `order_id`
- Other columns: `tracking_number`, `shipment_date`, `provider`, `shipment_status`

---

## 4. Summary of Keys

### **Primary Keys (PK)**
Each table has a unique column to identify its records:
- `product`: `product_id`
- `product_category`: `product_category_id`
- `product_brand`: `product_brand_id`
- `customer`: `customer_id`
- `customer_address`: `cust_address_id`
- `customer_payment`: `cust_payment_id`
- `order`: `order_id`
- `order_detail`: `order_detail_id`
- `order_payment`: `order_payment_id`
- `order_shipment`: `order_shipment_id`

### **Foreign Keys (FK)**
Foreign Keys establish relationships between tables:
- `product`: `product_category_id`, `product_brand_id`
- `customer_address`: `customer_id`
- `customer_payment`: `customer_id`
- `order`: `customer_id`
- `order_detail`: `order_id`, `product_id`
- `order_payment`: `order_id`
- `order_shipment`: `order_id`

---

## 5. Entity Relationship Diagram (ERD)

### Visual Representation
Below is the ERD for the Zolara Database, showcasing the relationships between its tables.

```mermaid
erDiagram
    customer {
        int customer_id PK
        string username
        string first_name
        string last_name
        string email
        string phone
    }
    customer_address {
        int cust_address_id PK
        int customer_id FK
        string address
        string province
        string city
        string district
        string postal_code
        string phone
    }
    customer_payment {
        int cust_payment_id PK
        int customer_id FK
        string payment_type
        string provider
        string account_no
        date expire_date
    }
    order {
        int order_id PK
        int customer_id FK
        string order_status
        timestamp created
    }
    order_detail {
        int order_detail_id PK
        int order_id FK
        int product_id FK
        numeric quantity
        numeric total_price
    }
    order_payment {
        int order_payment_id PK
        int order_id FK
        numeric payment_amount
        string payment_type
        string provider
        timestamp payment_date
        string payment_status
    }
    order_shipment {
        int order_shipment_id PK
        int order_id FK
        string tracking_number
        timestamp shipment_date
        string provider
        string shipment_status
    }
    product {
        int product_id PK
        int product_category_id FK
        int product_brand_id FK
        string name
        text description
        numeric price
        numeric stock
        bytea image
    }
    product_category {
        int product_category_id PK
        string name
        text description
    }
    product_brand {
        int product_brand_id PK
        string name
        text description
    }

    customer ||--o{ customer_address : "has"
    customer ||--o{ customer_payment : "has"
    customer ||--o{ order : "places"
    order ||--o{ order_detail : "contains"
    order ||--|| order_payment : "has"
    order ||--|| order_shipment : "has"
    order_detail ||--o{ product : "references"
    product ||--o{ product_category : "belongs to"
    product ||--o{ product_brand : "belongs to"

## 6. Conclusion
The Zolara Database's relationships ensure efficient data management by:

Reducing redundancy through normalized tables.
Maintaining data integrity using Foreign Keys.
Allowing advanced querying capabilities for better data insights.
