### ** Joins**
Joins are used to combine data from two or more tables based on related columns.

#### **Types of Joins:**
- **INNER JOIN:** Returns records with matching values in both tables.
  ```sql
  SELECT customers.nama_customer, payments.jumlah_pembayaran
  FROM customers
  INNER JOIN payments ON customers.id_customer = payments.id_customer;
  ```
  **Output:** This query retrieves customer names and their payments.

- **LEFT JOIN (OUTER JOIN):** Returns all records from the left table and matching records from the right table.
  ```sql
  SELECT customers.nama_customer, payments.jumlah_pembayaran
  FROM customers
  LEFT JOIN payments ON customers.id_customer = payments.id_customer;
  ```
  **Output:** This query retrieves all customers and their payments (including customers with no payments).

- **RIGHT JOIN:** Returns all records from the right table and matching records from the left table.
- **FULL OUTER JOIN:** Returns all records when there is a match in either table.

---
