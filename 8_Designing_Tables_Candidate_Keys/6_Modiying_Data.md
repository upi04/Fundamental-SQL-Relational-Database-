# Module: Inserting, Modifying, and Deleting Data (DML)

This module explains **Data Manipulation Language (DML)** commands in SQL: how to **insert** new records, **update** existing records, and **delete** unwanted records from a table. It uses examples from a hypothetical `product_category` table in a PostgreSQL database.

---

## 1. Overview: DML Commands

### 1.1. `INSERT`
- Adds new rows to a table.

### 1.2. `UPDATE`
- Modifies existing rows in a table.

### 1.3. `DELETE`
- Removes existing rows from a table.

All these statements typically involve a **WHERE** clause to filter which rows to manipulate.

---

## 2. Inserting Data

### 2.1. Scenario: `product_category` Table

**Schema Example**:
- **product_category_id**: integer (primary key, possibly `SERIAL`)
- **name**: varchar(255)
- **description**: text

#### 2.1.1. Case 1: Insert a Single Row (Specifying All Columns)

```sql
INSERT INTO product_category
VALUES (1, 'Shirt', 'A shirt is a piece of clothing worn on the upper part of your body with a collar, sleeves, and buttons down the front.');
```


Thought for a second
markdown
Copy code
# Module: Inserting, Modifying, and Deleting Data (DML)

This module explains **Data Manipulation Language (DML)** commands in SQL: how to **insert** new records, **update** existing records, and **delete** unwanted records from a table. It uses examples from a hypothetical `product_category` table in a PostgreSQL database.

---

## 1. Overview: DML Commands

### 1.1. `INSERT`
- Adds new rows to a table.

### 1.2. `UPDATE`
- Modifies existing rows in a table.

### 1.3. `DELETE`
- Removes existing rows from a table.

All these statements typically involve a **WHERE** clause to filter which rows to manipulate.

---

## 2. Inserting Data

### 2.1. Scenario: `product_category` Table

**Schema Example**:
- **product_category_id**: integer (primary key, possibly `SERIAL`)
- **name**: varchar(255)
- **description**: text

#### 2.1.1. Case 1: Insert a Single Row (Specifying All Columns)

```sql
INSERT INTO product_category
VALUES (1, 'Shirt', 'A shirt is a piece of clothing worn on the upper part of your body with a collar, sleeves, and buttons down the front.');
No column names specified here, which means the values must match the exact order of columns in the table definition.
#### 2.1.2. Case 2: Insert a Row Without ID
If product_category_id is auto-increment (SERIAL), you don’t need to supply the ID:
```sql
INSERT INTO product_category (name, description)
VALUES ('T-shirt', 'T-shirt has short sleeves and a round neckline, known as a crew neck, which lacks a collar.');
```
Or use DEFAULT:

```sql

INSERT INTO product_category (product_category_id, name, description)
VALUES (DEFAULT, 'T-shirt', 'T-shirt has short sleeves and a round neckline, known as a crew neck, which lacks a collar.');
```
#### 2.1.3. Case 3: Insert Multiple Rows
You can insert multiple rows in one statement by separating each row’s values with commas:

```sql

INSERT INTO product_category (name, description)
VALUES 
('Trousers', 'Trouser is an outer garment covering the lower half of the body from the waist to the ankles.'),
('Skirt', 'A skirt is the lower part of a dress or a separate outer garment that covers a person from the waist downwards.'),
('Outerwear', 'Outerwear is clothing and accessories worn outdoors, or clothing designed to be worn outside other garments.');
```
### 2.2. General Syntax for Inserting
```sql
Copy code
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```
You can list columns in any order, but the VALUES must match that order.
If a column is auto-increment (SERIAL), you can either omit it entirely or use DEFAULT.
## 3. Modifying (Updating) Data
### 3.1. Scenario: Changing a Product Category Name
We have a row with name = 'Trousers', and we want to change it to 'Pants'.

Before Update (sample):

product_category_id	name	description
1	Shirt	A shirt is a piece of clothing worn ...
2	T-shirt	T-shirt has short sleeves ...
3	Trousers	Trouser is an outer garment covering ...
4	Skirt	A skirt is the lower part of a dress ...
5	Outwear	Outerwear is clothing and accessories ...
#### 3.1.1. Updating the Column
```sql

UPDATE product_category
SET name = 'Pants'
WHERE name = 'Trousers';
```
UPDATE product_category: Tells the database we’re updating the product_category table.
SET name = 'Pants': We change the name column’s value.
WHERE name = 'Trousers': Only rows matching name = 'Trousers' are updated.
After Update:

product_category_id	name	description
1	Shirt	A shirt is a piece of clothing worn ...
2	T-shirt	T-shirt has short sleeves ...
3	Pants	Trouser is an outer garment covering ...
4	Skirt	A skirt is the lower part of a dress ...
5	Outwear	Outerwear is clothing and accessories ...
### 3.2. General Syntax for Updating
```sql
Copy code
UPDATE table_name
SET column1 = value1,
    column2 = value2
WHERE condition;
```
Omitting WHERE updates all rows, so use caution.
## 4. Deleting Data
### 4.1. Scenario: Removing a Product Category
Before Delete:

product_category_id	name	description
1	Shirt	A shirt is a piece of clothing worn ...
2	T-shirt	T-shirt has short sleeves ...
3	Trousers	Trousers is an outer garment covering ...
4	Skirt	A skirt is the lower part of a dress ...
5	Outwear	Outerwear is clothing and accessories ...
We want to remove the row where name = 'Outwear'.

####4.1.1. Deletion Statement
```sql

DELETE FROM product_category
WHERE name = 'Outwear';
```
DELETE FROM product_category: We’re deleting rows from the product_category table.
WHERE name = 'Outwear': Only rows matching name = 'Outwear' are removed.
After Delete:

product_category_id	name	description
1	Shirt	A shirt is a piece of clothing worn ...
2	T-shirt	T-shirt has short sleeves ...
3	Trousers	Trousers is an outer garment covering ...
4	Skirt	A skirt is the lower part of a dress ...
### 4.2. General Syntax for Deleting
``` sql

DELETE FROM table_name
WHERE condition;
```
If WHERE is omitted, all rows in the table are deleted.
## 5. Summary of Syntax
Insert Data:

``sql

INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```
Update Data:

```sql

UPDATE table_name
SET column1 = value1,
    column2 = value2
WHERE condition;
```
Delete Data:

```sql

DELETE FROM table_name
WHERE condition;
```
## 6. Conclusion
Inserting data (INSERT) allows you to add new rows, either specifying or omitting auto-increment IDs.
Modifying data (UPDATE) changes existing rows based on specific conditions—use WHERE to avoid affecting all rows unintentionally.
Deleting data (DELETE) removes rows that match a specified condition.