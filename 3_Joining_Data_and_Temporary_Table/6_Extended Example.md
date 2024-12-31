###  Extended Examples: Additional SQL Cases
## DataSet from (car_retails_postgres.sql)

#### Case 1: Products Not Sold
```sql
SELECT * 
FROM products
LEFT JOIN orderdetails
	USING(productCode)
WHERE orderNumber IS NULL;
```
**Output:** This query retrieves products that have not been sold.

#### Case 1.2: Products Sold Below 20% Stock
```sql
WITH total_sales AS (
    SELECT productCode, SUM(quantityOrdered) n_items_sold
    FROM orderdetails
    GROUP BY productCode)
SELECT productName,
       quantityInStock,
       n_items_sold,
       n_items_sold + quantityInStock total_quantity,
       n_items_sold::float / (n_items_sold + quantityInStock) * 100 percent_items_sold
FROM total_sales
JOIN products USING(productCode)
ORDER BY 5 ASC;
```
**Output:** This query identifies products sold below 20% of their total stock.

#### Case 1.3: Products Priced Below Vendor Recommendation
```sql
SELECT 
    productName, 
    MSRP,
    priceEach,
    0.8*MSRP harga_minimum,
    ((priceEach)/MSRP)*100 percent_priceEach_MSRP
FROM products
JOIN orderdetails USING(productCode)
WHERE priceEach < MSRP*0.8;
```
**Output:** This query retrieves products sold at prices 20% below the vendor's recommended price.

#### Case 1.4: Sales Categories Above Average
```sql
WITH total_each_product AS (
    SELECT productcode, SUM(quantityOrdered*priceEach) total_price
    FROM orderdetails
    GROUP BY 1
), total_each_category AS (
    SELECT productline, SUM(total_price) AS total_sales_category
    FROM total_each_product
    JOIN products USING(productcode)
    GROUP BY 1)
SELECT *
FROM total_each_category
WHERE total_sales_category > (SELECT AVG(total_sales_category) FROM total_each_category);
```
**Output:** This query retrieves product categories with sales above the average category sales.

#### Case 2.1: Average Payments Per Customer
```sql
SELECT customerName, AVG(amount) rata2Payments
FROM customers
JOIN payments USING(customerNumber)
GROUP BY customerName
ORDER BY 2 DESC;
```
**Output:** This query calculates the average payment amount for each customer.

#### Case 2.2: Customer Order Information
```sql
SELECT customerName,
       productName,
       SUM(quantityOrdered) jumlah_produk
FROM customers
JOIN orders USING(customerNumber)
JOIN orderDetails USING(orderNumber)
JOIN products USING(productCode)
GROUP BY customerName, productName;
```
**Output:** This query retrieves the total quantity of products ordered by each customer.

#### Case 2.3: Customers Exceeding Average Payments in Specific Countries
```sql
WITH total_payments_each_customer AS (
    SELECT customerNumber, SUM(amount) totalPayments
    FROM payments
    GROUP BY 1)
SELECT customerNumber, customerName, country, totalPayments
FROM total_payments_each_customer
JOIN customers USING(customerNumber)
WHERE totalPayments > (SELECT AVG(totalPayments) FROM total_payments_each_customer)
  AND country IN ('New Zealand', 'Australia', 'Singapore', 'Japan', 'Hong Kong', 'Philippines');
```
**Output:** This query retrieves customers exceeding average payments in specific countries.

#### Case 2.4: Top 5 Employees by Sales in 2004
```sql
WITH topSales2004 AS (
    SELECT salesRepEmployeeNumber employeeNumber,
           SUM(priceEach * quantityOrdered) total_sales
    FROM customers
    JOIN orders USING(customerNumber)
    JOIN orderdetails USING(orderNumber)
    WHERE EXTRACT(YEAR FROM orderDate) = 2004 AND status = 'Shipped'
    GROUP BY employeeNumber
    ORDER BY 2 DESC
    LIMIT 5)
SELECT CONCAT(firstName, ' ', lastName) employeeName, 
       total_sales
FROM topSales2004
JOIN employees USING(employeeNumber);
```
**Output:** This query retrieves the top 5 employees by sales in 2004.

---

### Summary of Concepts
- **Relations in Database:** Tables are connected using primary and foreign keys.
- **Joins:** Combine data from multiple tables (INNER, LEFT, RIGHT, FULL).
- **Subqueries:** Nested queries for filtering or deriving intermediate results.
- **CTEs:** Named temporary result sets for easier query structuring.
- **Temporary Tables:** Store and manipulate data temporarily during a session.
- **Extended Examples:** Advanced SQL cases demonstrating joins, subqueries, CTEs, and temporary tables in practical use cases.
