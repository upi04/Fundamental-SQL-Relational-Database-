# Candidate Keys in Database Design

This module discusses **candidate keys**, **composite candidate keys**, **artificial candidate keys**, and how they affect database design. It also includes examples demonstrating how to identify candidate keys within specific tables.

---

## 1. Why Are Keys Important?

- **Keys** ensure each record in a table can be uniquely identified.
- They form **relationships** (via foreign keys) between tables.
- They guarantee uniqueness and consistency of data across the database.

### Definition: Key
A **key** is a special field in a database table that has a **specific role**, such as providing unique identification for each record.

---

## 2. Types of Keys

1. **Primary Key (PK)**  
   - A unique identifier for every record in a table.

2. **Foreign Key (FK)**  
   - A link to a primary key in another table, establishing relationships between tables.

3. **Candidate Key (CK)**  
   - **All fields** that could serve as a primary key.  
   - A candidate key can be a **single** field or a **combination** of fields.

4. **Composite Candidate Key (CCK)**  
   - A **combination** of two or more fields that together form a candidate key.

5. **Artificial (Surrogate) Key (ACK)**  
   - A **new field** introduced to serve as a primary key when no suitable natural candidate key exists.

---

## 3. Example: Determining Candidate Keys

### 3.1. Employee Table

Consider the following **Employee** table:

| employee_id | ssn       | firstName | lastName | streetAddress      | city       | state | zipcode | homePhone |
|-------------|-----------|-----------|----------|--------------------|-----------|-------|---------|----------|
| 1000        | 987659938 | Kira      | Bently   | 1204 Bryant Road  | Seattle   | WA    | 98157   | 221363   |
| 1001        | 987656531 | Katherine | Erlich   | 101 C Street      | Bellevue  | WA    | 98046   | 251333   |
| 1002        | 987650399 | Bill      | Chamlin  | 7402 Kingman      | Redmond   | WA    | 98115   | 221222   |
| 1003        | 987651299 | Shannon   | Black    | 4141 Lake City    | Seattle   | WA    | 98136   | 221369   |
| 1004        | 987656529 | Katherine | Black    | 2100 Mineola Ave  | Seattle   | WA    | 98115   | 220360   |

#### Potential Keys in `Employee` Table

- **employee_id**:  
  - Each employee has a unique ID.  
  - Cannot be null, does not repeat → satisfies candidate key requirements.
  
- **ssn** (Social Security Number):  
  - Each employee typically has a unique SSN.  
  - Cannot be null, does not repeat → also satisfies candidate key requirements.

**Therefore, `employee_id` and `ssn` are both candidate keys.**

#### Other Fields

- **firstName**, **lastName**, **streetAddress**, **city**, **zipCode**, **homePhone**:  
  - These can repeat or be null (e.g., employees might share the same last name, some might not have a home phone).
  - They do **not** satisfy candidate key requirements.

Hence:

| Field        | Key Classification    |
|--------------|-----------------------|
| employee_id  | **CK** (Candidate Key)|
| ssn          | **CK** (Candidate Key)|
| firstName    | -                     |
| lastName     | -                     |
| streetAddress| -                     |
| city         | -                     |
| zipCode      | -                     |
| homePhone    | - (can be null)       |

---

### 3.2. Composite Candidate Key (CCK)

A **Composite Candidate Key** arises when no single field can guarantee uniqueness, but a **combination** of fields can.

**Example**: `(firstName, lastName)` might be unique if the company ensures no two employees share the exact same first+last name. However, in practice, this is risky because it’s possible to have two “John Smith.” So typically, `(firstName, lastName)` is **not** a reliable composite candidate key in many real-world scenarios. But in a hypothetical system, if the business rules guaranteed uniqueness, it could serve as a CCK.

---

## 4. Artificial (Surrogate) Keys

Sometimes, **none** of the existing fields can uniquely identify a record without the possibility of duplicates or nulls. In such cases, we introduce a **new** field that acts as a primary key—often an auto-increment integer or a universally unique identifier (UUID).

### 4.1. Example: `part` Table

| part_name          | model_number | manufacturer_name    | retail_price |
|--------------------|-------------|----------------------|-------------:|
| Shimka XT Cranks   | XT-113      | Shimka Incorporated  |       199.95 |
| Faust Brake Levers | XL/45       | Faust USA            |        53.79 |
| MiniMite Pump      | MiniMite    | MiniMite             |        35.00 |
| Hobo Fanny Pack    | -           | Hobo Bike Company    |         5.00 |

- `part_name` and `model_number` might **repeat** or be null/`-`.  
- `manufacturer_name` and `retail_price` can also be duplicated.  

No combination of these fields guarantees uniqueness or non-null. Therefore, we can create an **artificial key** (e.g., `part_id`) that becomes the **candidate key** and ultimately the **primary key**.

**Revised `part` Table**:

| part_id | part_name          | model_number | manufacturer_name    | retail_price |
|--------:|--------------------|-------------:|----------------------|-------------:|
| 1       | Shimka XT Cranks   | XT-113       | Shimka Incorporated  |       199.95 |
| 2       | Faust Brake Levers | XL/45        | Faust USA            |        53.79 |
| 3       | MiniMite Pump      | MiniMite     | MiniMite             |        35.00 |
| 4       | Hobo Fanny Pack    | -            | Hobo Bike Company    |         5.00 |

- `part_id` becomes the **artificial candidate key** (ACK) because it’s guaranteed to be unique, non-null, and stable.

---

## 5. Summary of Key Points

1. **Candidate Key** (CK): Any field(s) that can **uniquely identify** a record.  
2. **Composite Candidate Key** (CCK): A **combination** of two or more fields that collectively ensure uniqueness.  
3. **Artificial (Surrogate) Key** (ACK): A new field created to serve as the primary key when no natural candidate key exists.

### General Steps for Finding CKs

1. **Check each field** for uniqueness and non-null properties.  
2. **Check combinations of fields** (composite) if no single field suffices.  
3. **If no natural combination works**, consider creating an **artificial key**.

---

## 6. Reference Tables

Below are the **example tables** discussed:

### 6.1 Employee Table

| employee_id (CK) | ssn (CK)  | firstName | lastName | streetAddress      | city     | state | zipcode | homePhone |
|------------------|-----------|----------|----------|--------------------|---------|-------|---------|----------|
| 1000             | 987659938 | Kira     | Bently   | 1204 Bryant Road   | Seattle | WA    | 98157   | 221363   |
| 1001             | 987656531 | Katherine| Erlich   | 101 C Street       | Bellevue| WA    | 98046   | 251333   |
| 1002             | 987650399 | Bill     | Chamlin  | 7402 Kingman       | Redmond | WA    | 98115   | 221222   |
| 1003             | 987651299 | Shannon  | Black    | 4141 Lake City     | Seattle | WA    | 98136   | 221369   |
| 1004             | 987656529 | Katherine| Black    | 2100 Mineola Avenue| Seattle | WA    | 98115   | 220360   |

### 6.2 Part Table (Before Artificial Key)

| part_name          | model_number | manufacturer_name    | retail_price |
|--------------------|-------------:|----------------------|-------------:|
| Shimka XT Cranks   | XT-113       | Shimka Incorporated  |       199.95 |
| Faust Brake Levers | XL/45        | Faust USA            |        53.79 |
| MiniMite Pump      | MiniMite     | MiniMite             |        35.00 |
| Hobo Fanny Pack    | -            | Hobo Bike Company    |         5.00 |

#### After Introducing `part_id` (Artificial Key)

| part_id | part_name          | model_number | manufacturer_name    | retail_price |
|--------:|--------------------|-------------:|----------------------|-------------:|
| 1       | Shimka XT Cranks   | XT-113       | Shimka Incorporated  |       199.95 |
| 2       | Faust Brake Levers | XL/45        | Faust USA            |        53.79 |
| 3       | MiniMite Pump      | MiniMite     | MiniMite             |        35.00 |
| 4       | Hobo Fanny Pack    | -            | Hobo Bike Company    |         5.00 |

---

## 7. Conclusion

- **Candidate Key** ensures **unique identification** of each row in a table.  
- **Multiple candidate keys** may exist (e.g., `employee_id` and `ssn`).  
- **Composite** keys might be needed if no single field can guarantee uniqueness.  
- **Artificial (surrogate) key** is introduced when no natural candidate key works.

A well-chosen key maintains **data integrity** and **simplifies relationships** in your database design, forming the backbone of efficient queries and robust referential integrity.
