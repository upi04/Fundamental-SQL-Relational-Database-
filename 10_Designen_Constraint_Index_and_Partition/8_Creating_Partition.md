
# Creating Partition in PostgreSQL

## Introduction to Partitioning
Partitioning is the process of breaking a large table into smaller, more manageable pieces to improve query performance and optimize database storage.

### Advantages of Partitioning:
1. **Performance Boost**: Reduces query time by narrowing down the scope of data scanned.
2. **Easier Maintenance**: Simplifies data management for large datasets.

### Challenges:
1. **Complexity**: Requires careful design and planning.
2. **Hard to Modify**: Changes to partitioning strategy can be cumbersome.

---

## Types of Partitioning

### Horizontal Partitioning
Splits a table based on row values.  
Example: Splitting a `sales` table by transaction year.

| Partition A | Orders before 2020 |
|-------------|---------------------|
| Partition B | Orders from 2020    |

### Vertical Partitioning
Splits a table based on columns.  
Example: Separating customer contact details into a separate partition.

| Partition A | id, username, email, phone |
|-------------|-----------------------------|
| Partition B | address, city, country      |

---

## Creating List Partition
### Example: Partitioning a `sales_region` table by region.

#### Partitioned Table
```sql
CREATE TABLE sales_region (
    sales_region_id INT,
    amount NUMERIC,
    branch TEXT,
    region TEXT
)
PARTITION BY LIST (region);
```

#### Partitions
- Jakarta
```sql
CREATE TABLE Jakarta
PARTITION OF sales_region
FOR VALUES IN ('Jakarta');
```
- Bandung
```sql
CREATE TABLE Bandung
PARTITION OF sales_region
FOR VALUES IN ('Bandung');
```
- Surabaya
```sql
CREATE TABLE Surabaya
PARTITION OF sales_region
FOR VALUES IN ('Surabaya');
```

---

## Creating Range Partition
### Example: Partitioning a `sales` table by quarter.

#### Partitioned Table
```sql
CREATE TABLE sales (
    sales_id INT,
    product_name TEXT,
    amount INT,
    sale_date DATE
)
PARTITION BY RANGE (sale_date);
```

#### Partitions
- Q1: January - March 2020
```sql
CREATE TABLE sales_2020_Q1
PARTITION OF sales
FOR VALUES FROM ('2020-01-01') TO ('2020-04-01');
```
- Q2: April - June 2020
```sql
CREATE TABLE sales_2020_Q2
PARTITION OF sales
FOR VALUES FROM ('2020-04-01') TO ('2020-07-01');
```
- Q3: July - September 2020
```sql
CREATE TABLE sales_2020_Q3
PARTITION OF sales
FOR VALUES FROM ('2020-07-01') TO ('2020-10-01');
```
- Q4: October - December 2020
```sql
CREATE TABLE sales_2020_Q4
PARTITION OF sales
FOR VALUES FROM ('2020-10-01') TO ('2021-01-01');
```

---

## Partition Syntax Summary
### Defining Partitioned Table
```sql
CREATE TABLE table_name (
    column1 data_type,
    column2 data_type,
    ...
)
PARTITION BY [LIST | RANGE](partition_column);
```

### Creating Partitions
- **List Partition**:
```sql
CREATE TABLE partition_name
PARTITION OF table_name
FOR VALUES IN (value1, value2, ...);
```

- **Range Partition**:
```sql
CREATE TABLE partition_name
PARTITION OF table_name
FOR VALUES FROM (start_value) TO (end_value);
```

---

## Best Practices
1. Partition large tables only when necessary.
2. Carefully plan the partitioning strategy based on query patterns.
3. Use tools like `EXPLAIN` to validate query performance improvements.

