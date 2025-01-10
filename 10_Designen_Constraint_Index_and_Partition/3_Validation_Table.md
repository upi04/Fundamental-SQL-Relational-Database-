# Validation Table and Constraints Module

## 1. Introduction

This module explains the use of **Validation Tables** and **Constraints** to implement efficient business rules in database management. Validation tables help ensure data consistency by restricting data entry to predefined values.

---

## 2. Check Constraint for Specific Values

### Case:
An application only serves customers residing in the following cities:
- Jakarta Pusat
- Jakarta Utara
- Jakarta Timur
- Jakarta Barat
- Jakarta Selatan
- Bandung
- Surabaya
- Semarang
- Yogyakarta

To enforce this rule, a `CHECK` constraint can be added to the `city` column of the `customer_address` table.

### SQL Implementation:
```sql
ALTER TABLE customer_address 
ADD CONSTRAINT city_check 
CHECK (
    city IN (
        'Jakarta Pusat', 'Jakarta Utara', 'Jakarta Timur',
        'Jakarta Barat', 'Jakarta Selatan', 'Bandung',
        'Surabaya', 'Semarang', 'Yogyakarta'
    )
);
```
Steps:
Use the ALTER TABLE command followed by the table name (customer_address).
Add a new constraint using the ADD CONSTRAINT command and name the constraint (city_check).
Use the CHECK keyword to define the condition. Specify the column (city) and the valid values using the IN clause.
## 3. Validation Table
If the list of valid values is too long, checking the condition for each insert operation can become inefficient. To solve this, use a Validation Table.

Definition:
A Validation Table is a table used to store predefined, relatively static values that serve as a reference for valid data entries.

Example: City Validation Table
city_id	name
3171	Jakarta Pusat
3172	Jakarta Utara
3173	Jakarta Barat
3174	Jakarta Selatan
3175	Jakarta Timur
3578	Surabaya
3471	Yogyakarta
## 4. Steps to Create a Validation Table
4.1 Create Validation Table city
```sql

CREATE TABLE city(
    city_id INT PRIMARY KEY,
    name VARCHAR(100)
);
city_id: The primary key of the table.
name: The city name.
### 4.2 Modify the customer_address Table
Step 1: Drop the city Column
``` sql

ALTER TABLE customer_address 
DROP COLUMN city;
Use the ALTER TABLE command followed by the table name.
Use DROP COLUMN to remove the city column.
```
Step 2: Add a New city_id Column
```sql

ALTER TABLE customer_address 
ADD COLUMN city_id INTEGER;
Use the ALTER TABLE command followed by the table name.
Use ADD COLUMN to add the city_id column to the table.
```
Step 3: Add a Foreign Key Constraint
```sql

ALTER TABLE customer_address 
ADD CONSTRAINT fk_address_city 
FOREIGN KEY(city_id) 
REFERENCES city(city_id);
Use the ADD CONSTRAINT command to create a new constraint named fk_address_city.
Use the FOREIGN KEY keyword followed by the city_id column.
Use the REFERENCES keyword to link the city_id column to the city table.
```
## 5. Relationship Between Tables
The relationship between the customer_address table and the city table is established as follows:

customer_address Table:
Field	Type	Key
cust_address_id	INT	PK
customer_id	INT	FK
address	TEXT	
province	VARCHAR(255)	
city_id	INT	FK
postal_code	VARCHAR(10)	
phone	VARCHAR(20)	
city Table:
Field	Type	Key
city_id	INT	PK
name	VARCHAR(100)	
## 6. Data Migration Steps
If the customer_address table already has records, follow these steps to migrate data to the new structure:

Export Data:

Export existing data from the customer_address and city tables.
Match Existing Data:

Map customer_address.city values to city.name values to ensure consistent city_id values.
Insert Data:

Insert data back into the customer_address table with the city_id column referencing the city table.
Verify Data:

Use SQL queries to verify the integrity of the updated data.
## 7. Benefits of Validation Table
Efficiency: Improves the speed of insert operations by avoiding repetitive CHECK conditions.
Consistency: Centralizes valid values in one table, ensuring uniformity.
Flexibility: Allows easy updates to the list of valid values.
## 8. Conclusion
Using Validation Tables and Constraints is a best practice in database design for implementing efficient and consistent business rules. This module provides a step-by-step guide to creating and modifying validation tables, ensuring data integrity and ease of maintenance.






