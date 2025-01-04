### Aggregation
Aggregation functions are used to summarize data, such as calculating totals or averages.

#### **Example 1**: Calculate the total salary of all employees
```sql
SELECT SUM(salary) AS total_salary
FROM employee;
```
**Output**: This query calculates the sum of all employee salaries.

#### **Example 2**: Find the average salary of employees
```sql
SELECT AVG(salary) AS average_salary
FROM employee;
```
**Output**: This query calculates the average salary of all employees.

---

### Grouping
Grouping is used to organize data into groups for aggregation.

#### **Example 1**: Find the total salary by branch
```sql
SELECT branch_id, SUM(salary) AS total_salary
FROM employee
GROUP BY branch_id;
```
**Output**: This query calculates the total salary for each branch.

#### **Example 2**: Count the number of employees in each branch
```sql
SELECT branch_id, COUNT(emp_id) AS employee_count
FROM employee
GROUP BY branch_id;
```
**Output**: This query counts the number of employees in each branch.

### **Aggregation Examples with Customers and Payments**

#### **Example 1**: Calculate the total payments made by each customer
```sql
SELECT id_customer, SUM(jumlah_pembayaran) AS total_payments
FROM payments
GROUP BY id_customer;
```
**Output**: This query calculates the total payment amount for each customer.

#### **Example 2**: Find the average payment amount
```sql
SELECT AVG(jumlah_pembayaran) AS average_payment
FROM payments;
```
**Output**: This query calculates the average payment amount.

