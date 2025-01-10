# Partitioning in PostgreSQL

## 1. Introduction to Partitioning

**Partitioning** is the process of dividing a large table into smaller, more manageable parts. These smaller parts are called **partitions**, and they help improve database performance by reducing query times and memory usage.

---

## 2. Why Use Partitioning?

### Scenario:
Consider a company with thousands of orders every day. Over time, the `orders` table grows so large that queries become slow, affecting performance. The solution is to split the table into smaller partitions, such as by transaction date.

### Benefits of Partitioning:
1. **Performance Boost**: Faster query execution by targeting specific partitions.
2. **Painless Maintenance**: Easier management of data (e.g., archiving older partitions).

### Drawbacks of Partitioning:
1. **Complexity**: Increases the complexity of database design.
2. **Hard to Change**: Partitions must be defined during table creation and are challenging to modify later.

---

## 3. When to Use Partitioning?

- Use partitioning for **large tables** where performance is critical.
- Suitable for tables with **slow queries** due to their size.
- Tables requiring frequent analysis or querying on specific ranges or columns.

---

## 4. Declarative Partitioning in PostgreSQL

### Definition:
A **partitioned table** is a virtual table that does not store data directly. Instead, its data is stored in its partitions.

### How it Works:
- Queries on a partitioned table are automatically redirected to relevant partitions based on query conditions.
- Each partition has its own storage, separate from the parent partitioned table.

---

## 5. Types of Partitioning

### 5.1. Horizontal Partitioning
- Splits the table based on **row values**.
- Common use case: Splitting data based on date or category.

#### Example:
Partition an `orders` table by transaction year:
- **Partition A**: Orders before 2020 (`order_date < '2020-01-01'`)
- **Partition B**: Orders from 2020 and later (`order_date >= '2020-01-01'`)

#### Benefits:
- Queries can target specific date ranges, improving performance.

---

### 5.2. Vertical Partitioning
- Splits the table based on **columns**.
- Some columns are stored in one partition, while the rest are stored in another.

#### Example:
Split a `customers` table:
- **Partition A**: Includes `id`, `username`, `email`, and `phone`.
- **Partition B**: Includes other fields such as `address`, `city`, and `zip_code`.

#### Benefits:
- Reduces the size of each partition, making queries faster for frequently accessed columns.

---

