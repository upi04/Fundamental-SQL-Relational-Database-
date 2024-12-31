## **Additional Examples: Customer and Payment Data**

### **Customers and Payments Dataset**
The following dataset represents customers and their payments:

#### **Table: customers**
```sql
CREATE TABLE customers(
	id_customer INT,
    nama_customer VARCHAR(50),
    kota VARCHAR(50),
    PRIMARY KEY(id_customer)
);

INSERT INTO customers VALUES 
(1123, 'Reihan', 'Jakarta Barat'),
(1126, 'Abra', 'Depok'),
(1147, 'Beni', 'Jakarta Selatan'),
(1156, 'Lufi', 'Jakarta Selatan'),
(1189, 'Kaido', 'Depok');
```

#### **Table: payments**
```sql
CREATE TABLE payments(
	id_customer INT,
    id_transaksi INT,
    tgl_transaksi DATE,
    jumlah_pembayaran INT,
    PRIMARY KEY(id_transaksi)
);

INSERT INTO payments VALUES 
(1123, 1, '2021-09-01', 500000),
(1147, 2, '2021-09-01', 247000),
(1147, 3, '2021-09-02', 325000),
(1189, 4, '2021-09-03', 440000),
(1176, 5, '2021-09-03', 122000);
```

---

### **Case-Based Queries**

#### **Case 1**: Total sales and number of transactions per year
```sql
SELECT EXTRACT(YEAR FROM tgl_transaksi) AS tahun, 
       SUM(jumlah_pembayaran) AS total_sales, 
       COUNT(id_transaksi) AS jumlah_transaksi
FROM payments
GROUP BY tahun
ORDER BY tahun;
```
**Output**: This query calculates total sales and counts transactions for each year.

---

#### **Case 1.2**: Count customers with total transactions above and below average
```sql
WITH total_payment AS (
    SELECT id_customer, SUM(jumlah_pembayaran) AS total_amount
    FROM payments
    GROUP BY id_customer
),
category_cust AS (
    SELECT id_customer,
           CASE
               WHEN total_amount > (SELECT AVG(total_amount) FROM total_payment) THEN 'above_avg'
               ELSE 'below_avg'
           END AS category
    FROM total_payment
)
SELECT category, COUNT(id_customer) AS customer_count
FROM category_cust
GROUP BY category;
```
**Output**: This query categorizes customers based on their total payments and counts them by category.

---

#### **Case 1.3**: Categorize customer loyalty based on order count
```sql
WITH n_order_cust AS (
    SELECT id_customer, COUNT(id_transaksi) AS n_order
    FROM payments
    GROUP BY id_customer
)
SELECT id_customer, n_order,
       CASE
           WHEN n_order = 1 THEN 'One-time'
           WHEN n_order = 2 THEN 'Repeated'
           WHEN n_order = 3 THEN 'Frequent'
           ELSE 'Loyal'
       END AS category_customer
FROM n_order_cust;
```
**Output**: This query assigns a loyalty category to each customer based on their number of orders.

---

#### **Case 2.1**: Calculate average duration between customer orders
```sql
WITH next_order AS (
    SELECT id_customer, tgl_transaksi,
           LEAD(tgl_transaksi) OVER (PARTITION BY id_customer ORDER BY tgl_transaksi) AS next_order
    FROM payments
),
duration AS (
    SELECT id_customer, tgl_transaksi, next_order,
           next_order - tgl_transaksi AS duration_order
    FROM next_order
    WHERE next_order IS NOT NULL
)
SELECT id_customer, ROUND(AVG(duration_order), 0) AS avg_durasi
FROM duration
GROUP BY id_customer;
```
**Output**: This query calculates the average duration between customer orders.

---

#### **Case 2.2**: Retrieve first payment date and amount for each customer
```sql
WITH first_payment_date AS (
    SELECT id_customer, nama_customer, tgl_transaksi, jumlah_pembayaran,
           FIRST_VALUE(tgl_transaksi) OVER (PARTITION BY id_customer ORDER BY tgl_transaksi) AS first_payment_date,
           FIRST_VALUE(jumlah_pembayaran) OVER (PARTITION BY id_customer ORDER BY tgl_transaksi) AS first_payment_amount
    FROM customers
    JOIN payments USING(id_customer)
)
SELECT nama_customer, first_payment_date, first_payment_amount
FROM first_payment_date
GROUP BY nama_customer, first_payment_date, first_payment_amount
ORDER BY first_payment_date ASC;
```
**Output**: This query retrieves the first payment date and amount for each customer.

---

#### **Case 2.3**: Find the first and last customer order in each country
```sql
WITH first_last_customer AS (
    SELECT kota AS country, id_customer, tgl_transaksi,
           FIRST_VALUE(nama_customer) OVER (PARTITION BY kota ORDER BY tgl_transaksi) AS first_customer,
           LAST_VALUE(nama_customer) OVER (PARTITION BY kota ORDER BY tgl_transaksi ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_customer
    FROM customers
    JOIN payments USING(id_customer)
)
SELECT country, first_customer, last_customer
FROM first_last_customer
GROUP BY country, first_customer, last_customer;
```
**Output**: This query identifies the first and last customer orders in each country.

---

#### **Case 2.4**: Retrieve the second most expensive product ordered by each customer
```sql
WITH customers_products_ordered AS (
    SELECT id_customer, nama_customer, jumlah_pembayaran AS price_each
    FROM customers
    JOIN payments USING(id_customer)
),
second_most_expensive AS (
    SELECT id_customer, nama_customer,
           NTH_VALUE(jumlah_pembayaran, 2) OVER (PARTITION BY id_customer ORDER BY jumlah_pembayaran DESC) AS second_most_expensive
    FROM customers_products_ordered
)
SELECT id_customer, nama_customer, second_most_expensive
FROM second_most_expensive
WHERE second_most_expensive IS NOT NULL
GROUP BY id_customer, nama_customer, second_most_expensive;
```
**Output**: This query retrieves the second most expensive product ordered by each customer.
