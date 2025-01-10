# Database Indexing 


## 1. Introduction

Indexes are essential objects in databases that act as identifiers to improve data retrieval performance. They work similarly to the table of contents in a book, helping you quickly locate information without scanning through every page.

---

## 2. Understanding Indexes

### What is an Index?
An index is a database object that assigns an identifier to data within a table to enhance query performance.

### Advantages of Using Indexes:
- **Speed Up Queries**: Indexes reduce the time required to retrieve data.
- **Selective Searching**: Queries are executed on relevant data blocks, not the entire table.
- **Efficiency in Large Tables**: Indexes make large datasets manageable and queries faster.

---

## 3. Case Study: Searching Without and With an Index

### Without an Index:
When searching for `product_id = 2` in a `products` table:
- The database scans the entire table row by row until it finds the matching record.
- If the column is unique, the search stops after finding the first match.
- If the column is not unique, the search continues through all rows.

**Example Table:**
| product_id | name                 | price | stock |
|------------|----------------------|-------|-------|
| 1          | Chitato Jagung Bakar | 10000 | 100   |
| 2          | Paseo Tissue 50 Sheet| 5500  | 45    |
| 3          | Crystalline 600 ml   | 4000  | 90    |
| 4          | Prim-a 600 ml        | 3000  | 77    |

### With an Index:
- The database accesses only the relevant block where the matching data is located.
- This significantly reduces the search space and improves performance.

---

## 4. Types of Indexes

### Single Index:
- An index created on a single column.
- Example: `product_id` in the `products` table.

---

## 5. Choosing the Right Columns to Index

### Why Not Index Every Column?
- **Performance Impact**: Indexing every column increases the overhead for `INSERT`, `UPDATE`, and `DELETE` operations, as each modification requires updating the index.
- **Space Consumption**: Indexes duplicate the data structure, consuming additional storage.

### Tips for Selecting Index Columns:
1. **Focus on Frequently Queried Columns**:
   - Index columns often used in `WHERE`, `JOIN`, or `ORDER BY` clauses.
   - Example: Columns such as `product_id` or `name` in a frequently queried table.

2. **Selectivity**:
   - Index columns that return a small subset of data during queries.
   - Avoid indexing columns with low selectivity (e.g., boolean values).

3. **Parent-Child Relationships**:
   - If the primary key of a parent table is indexed, ensure the foreign key in the child table is also indexed.

---

## 6. Best Practices for Indexing

1. **Analyze Query Patterns**:
   - Understand the most common queries and identify the columns they involve.

2. **Limit the Number of Indexes**:
   - Use indexes sparingly to avoid excessive overhead.

3. **Combine Indexes**:
   - Use composite indexes for queries involving multiple columns.

4. **Monitor Index Performance**:
   - Regularly check for unused or inefficient indexes and optimize as needed.

---

## 7. Conclusion

Indexes are a powerful tool for improving database query performance. However, they must be used judiciously to balance query speed with data modification efficiency. By selecting the right columns to index and following best practices, you can optimize your database for both performance and scalability.
