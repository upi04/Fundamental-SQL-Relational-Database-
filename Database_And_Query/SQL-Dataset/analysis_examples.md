# **Basic SQL Queries**

## **1. Basic SELECT Statement**

### **Basic Structure**
```sql
SELECT column1, column2
FROM table_name;
```

### **Example 1**: Display `first_name` and `salary` from the `employee` table
```sql
SELECT first_name, salary
FROM employee;
```
**Output**: This query retrieves the first names and salaries of employees from the `employee` table.

---

## **2. Basic Filtering Data**

### **Using the WHERE Clause**

#### **Example 1**: Display employees with a `salary` greater than 60000
```sql
SELECT first_name, salary
FROM employee
WHERE salary > 60000;
```
**Output**: This query retrieves the first names and salaries of employees where the salary is greater than 60000.

---

#### **Example 2**: Search for employees whose `first_name` starts with the letter 'J'
```sql
SELECT first_name, last_name
FROM employee
WHERE first_name LIKE 'J%';
```
**Output**: This query retrieves the first and last names of employees where the first name starts with the letter 'J'.

---

#### **Example 3**: Combining Conditions
Find all employees with `salary > 60000` and `branch_id` equal to 2:
```sql
SELECT first_name, salary, branch_id
FROM employee
WHERE salary > 60000 AND branch_id = 2;
```
**Output**: This query retrieves the first names, salaries, and branch IDs of employees where the salary is greater than 60000 and they belong to branch ID 2.

---

