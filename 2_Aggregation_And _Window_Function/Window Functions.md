### **Window Functions**
Window functions perform calculations across a set of table rows related to the current row.

#### **Example 1**: Calculate the running total of employee salaries
```sql
SELECT first_name, salary, SUM(salary) OVER (ORDER BY emp_id) AS running_total
FROM employee;
```
**Output**: This query calculates the cumulative total salary, ordered by employee ID.

#### **Example 2**: Rank employees by their salary within each branch
```sql
SELECT first_name, branch_id, salary, RANK() OVER (PARTITION BY branch_id ORDER BY salary DESC) AS salary_rank
FROM employee;
```
**Output**: This query ranks employees by salary within each branch.


## **Window Function Examples with Customers and Payments**

#### **Example 1**: Calculate the running total of payments by transaction date
```sql
SELECT id_transaksi, tgl_transaksi, jumlah_pembayaran, 
       SUM(jumlah_pembayaran) OVER (ORDER BY tgl_transaksi) AS running_total
FROM payments;
```
**Output**: This query calculates the cumulative total of payments ordered by transaction date.

#### **Example 2**: Rank customers by their total payments
```sql
SELECT id_customer, SUM(jumlah_pembayaran) AS total_payments, 
       RANK() OVER (ORDER BY SUM(jumlah_pembayaran) DESC) AS payment_rank
FROM payments
GROUP BY id_customer;
```
**Output**: This query ranks customers based on their total payment amounts.

---
